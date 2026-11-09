# Reverse Proxy Lessons Learned: Correcting My Mental Model

## Background

As I expanded my homelab, I deployed several services behind Nginx Proxy Manager (NPM) including Grafana, Stirling PDF, Vaultwarden, Gitea, and various monitoring tools.

Although I had NPM working, I eventually realized that my understanding of where a reverse proxy sits in the traffic flow was incomplete. The result was that some services were technically "behind" NPM while still being directly reachable via their host IP and exposed ports.

This wasn't causing an outage, but it meant I wasn't using the reverse proxy as intentionally as I thought I was.

## Original Assumption

My mental model was:

```text
Client
  |
  v
NPM
  |
  v
Service
```

In practice, the architecture looked more like:

```text
Client
  |
  +--> https://service.arpa
  |         |
  |         v
  |       NPM
  |         |
  |         v
  |    Docker Service
  |
  +--> http://docker-host:port
```

The reverse proxy worked, but users could also bypass it completely by connecting directly to a service's exposed port.

For example:

```text
https://grafana.arpa
```

worked through NPM, but so did:

```text
http://10.4.4.x:3000
```

because Grafana's container port was also published directly to the Docker host.

## DNS Misunderstanding

I also discovered a mistake in how I was thinking about DNS.

For my Proxmox host, NPM was correctly configured:

```text
proxmox.arpa
    |
    v
NPM
    |
    v
10.x.x.x:8006
```

However, my OPNsense DNS override pointed directly to the Proxmox IP address rather than to NPM.

The result was:

```text
https://proxmox.arpa
```

failed because traffic never reached NPM.

Meanwhile:

```text
https://proxmox.arpa:8006
```

worked because I was connecting directly to Proxmox and bypassing the reverse proxy entirely.

After tracing the traffic path, the mistake became obvious:

```text
DNS must point to NPM,
not the backend service.
```

The reverse proxy can only proxy traffic that actually reaches it.

## Docker Networking Discovery

While reviewing my NPM configuration, I noticed that many services were configured like this:

```yaml
ports:
  - "8888:8080"
```

NPM was forwarding requests to:

```text
docker-host:8888
```

which meant traffic flowed:

```text
Client
  |
HTTPS
  |
NPM
  |
HTTP
  |
Docker Host Port
  |
Docker Container
```

This worked, but it wasn't necessary.

After learning more about Docker networking, I moved NPM and my services onto a shared Docker network and updated NPM to use service names instead of host ports.

Examples:

```text
stirling-pdf:8080
grafana:3000
vaultwarden:3000
```

Traffic now flows:

```text
Client
  |
HTTPS
  |
NPM
  |
Docker Network
  |
Service Container
```

## Removing Unnecessary Exposed Ports

Once NPM was communicating through Docker networking, I removed unnecessary port mappings from many containers.

Before:

```yaml
services:
  stirling-pdf:
    ports:
      - "8888:8080"
```

After:

```yaml
services:
  stirling-pdf:
    networks:
      - proxy
```

After redeploying:

```text
http://docker-host:8888
```

no longer worked, while:

```text
https://stirling.arpa
```

continued working normally through NPM.

This confirmed that NPM had become the intended entry point rather than just an optional convenience layer.

## Multi-Network Containers

While making these changes, I learned that a Docker container can belong to multiple networks.

For example:

```text
proxy
 ├─ NPM
 └─ Gitea

gitea_private
 ├─ Gitea
 └─ PostgreSQL
```

In this design:

- NPM can reach Gitea
- Gitea can reach PostgreSQL
- NPM cannot directly reach PostgreSQL

This was a useful lesson in separating application traffic from backend service traffic.

## Not Every Port Should Be Removed

I also realized that "behind a reverse proxy" does not mean every service should be hidden.

Some services still have legitimate reasons to expose ports:

- Memos, because my iOS client currently connects directly by IP and HTTP.
- iperf3, because it is intended for direct network performance testing.
- cAdvisor, because it is scraped externally by a Raspberry Pi running VictoriaMetrics.

The takeaway was not to eliminate every open port, but rather to be able to justify every open port.

## Final Architecture

Today, most web applications follow this pattern:

```text
Client
  |
  v
NPM (80/443)
  |
  +--> Grafana
  +--> Vaultwarden
  +--> Stirling PDF
  +--> Gitea
  +--> Other Web Applications
```

NPM communicates with local containers over Docker networking and can also proxy services hosted on different machines and VLANs.

Only services with a specific operational requirement remain directly exposed.

## Key Lessons Learned

- DNS entries for reverse proxied services should point to the reverse proxy, not the backend service.
- A reverse proxy only works if traffic actually reaches it.
- Docker containers can communicate over shared Docker networks without exposing ports to the host.
- Containers can belong to multiple Docker networks simultaneously.
- Publishing a port to the host is often unnecessary when a reverse proxy is present.
- The goal is not "zero open ports"; the goal is that every open port has a clear purpose.
- Understanding the intended traffic path is more important than simply making services reachable.

## Outcome

This project started as a simple effort to clean up some Nginx Proxy Manager entries and ended up correcting several networking assumptions I didn't realize I had.

The result was a cleaner Docker deployment, fewer exposed services, a better understanding of reverse proxies and Docker networking, and a more intentional approach to service exposure and network boundaries.

Most importantly, it reinforced a lesson I've encountered repeatedly while building this homelab:

> Working configurations are not always well-understood configurations.

Everything was functioning, but tracing the actual traffic paths exposed gaps in my mental model. Correcting those misunderstandings led to a cleaner design and a much better understanding of how DNS, reverse proxies, Docker networking, and service exposure fit together.
