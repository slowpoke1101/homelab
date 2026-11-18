# Deploy Centralized Logging On observer

## Purpose

Collect infrastructure syslog messages on observer, store them in Loki, and make them
available to Grafana. observer is the log collector; backup-node is reserved for backups.

## Prerequisites

- Docker Engine and Compose plugin installed on observer.
- observer has a management VLAN address and can receive UDP syslog on port 514.
- Firewall rules allow required nodes to send logs to observer.
- Persistent storage is available for logs and is included in backups.

## Receive Syslog With rsyslog

Install rsyslog on observer:

```bash
sudo apt update
sudo apt install rsyslog
```

Create `/etc/rsyslog.d/central.conf`:

```text
module(load="imudp")
input(type="imudp" port="514")

$template RemoteLogs,"/var/log/central/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& stop
```

Create the log directory, restart rsyslog, and confirm it is listening:

```bash
sudo mkdir -p /var/log/central
sudo systemctl restart rsyslog
sudo systemctl is-active rsyslog
sudo ss -lunp | grep ':514'
```

## Configure Log Sources

Configure each source to send syslog to observer's management IP on UDP port 514:

| Source | Configuration location |
| --- | --- |
| OPNsense | System > Settings > Logging > Remote |
| TrueNAS | System > Advanced Settings > Syslog |
| Proxmox | `/etc/rsyslog.conf` or a file under `/etc/rsyslog.d/` |
| Docker VM | `/etc/rsyslog.conf` or a file under `/etc/rsyslog.d/` |
| switch | Syslog settings in the switch web UI |
| Flint2 | Remote syslog settings, if supported |

For Linux sources, use:

```text
*.* @<OBSERVER_MANAGEMENT_IP>:514
```

Restart rsyslog on Linux sources after changing the configuration:

```bash
sudo systemctl restart rsyslog
```

Use the dedicated [Syslog Forwarding](syslog-forwarding-config.md) guide for source-specific
screens and configuration details.

## Deploy Loki And Alloy On observer

Create a Docker Compose project with persistent directories for Loki and Alloy:

```yaml
services:
  loki:
    image: grafana/loki:2.9.0
    container_name: loki
    restart: unless-stopped
    ports:
      - "3100:3100"
    volumes:
      - ./loki:/loki

  alloy:
    image: grafana/alloy:latest
    container_name: alloy
    restart: unless-stopped
    volumes:
      - /var/log/central:/var/log/central:ro
      - ./config.alloy:/etc/alloy/config.alloy:ro
    depends_on:
      - loki
```

Create `config.alloy`:

```text
logging {
  level = "info"
}

loki.write "local" {
  endpoint {
    url = "http://loki:3100/loki/api/v1/push"
  }
}

local.file_match "syslog" {
  path_targets = [{
    __path__ = "/var/log/central/*/*.log",
    job = "syslog",
    host = "observer",
  }]
}

loki.source.file "syslog" {
  targets    = local.file_match.syslog.targets
  forward_to = [loki.write.local.receiver]
}
```

Start the logging stack:

```bash
docker compose up -d
docker compose ps
docker compose logs --tail=50 alloy
```

## Validation

- Confirm rsyslog is listening on UDP 514.
- Generate or wait for a log from each configured source.
- Confirm files appear under `/var/log/central`.
- Confirm Loki is healthy at `http://localhost:3100/ready`.
- Query Loki from Grafana and verify recent infrastructure logs are visible.
- Confirm log storage is included in the observer backup plan.

## Recovery

If logs stop arriving, check the source configuration, firewall rule, rsyslog service, and
files under `/var/log/central` in that order. Do not expose the syslog or Loki ports beyond
the required management and monitoring networks.
