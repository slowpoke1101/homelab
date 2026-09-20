# Deploy Metrics Collection And Grafana

## Purpose

Deploy VictoriaMetrics on observer and Grafana on the Docker VM. VictoriaMetrics stores
metrics scraped from the homelab; Grafana provides the user-facing dashboards.

## Prerequisites

- Docker Engine and Compose plugin installed on observer and the Docker VM.
- observer can reach the node-exporter endpoints on OPNsense, TrueNAS, Proxmox, and the Docker VM.
- The Docker VM can reach VictoriaMetrics on observer.
- Create persistent directories and back them up before deployment.

## Deploy VictoriaMetrics On observer

```bash
sudo mkdir -p /srv/observ/victoriametrics
sudo chown -R "$USER":"$USER" /srv/observ/victoriametrics
cd /srv/observ/victoriametrics
```

Create `prometheus.yml` and replace `<observer-management-ip>` only where required by your
live configuration. The node addresses below are the documented current addresses.

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'docker-vm'
    static_configs:
      - targets: ['10.4.4.4:9100']
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['10.4.4.4:8086']
  - job_name: 'proxmox-node'
    static_configs:
      - targets: ['10.1.11.6:9100']
  - job_name: 'truenas-node'
    static_configs:
      - targets: ['10.1.11.7:9100']
  - job_name: 'opnsense-node'
    static_configs:
      - targets: ['<opnsense-metrics-ip>:9100']
```

Create `docker-compose.yml`:

```yaml
services:
  victoriametrics:
    image: victoriametrics/victoria-metrics:latest
    container_name: victoriametrics
    restart: unless-stopped
    ports:
      - "8428:8428"
    volumes:
      - ./data:/victoria-metrics-data
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
    command:
      - "--promscrape.config=/etc/prometheus/prometheus.yml"
```

Start the collector:

```bash
docker compose up -d
curl http://localhost:8428/health
```

## Deploy Grafana On The Docker VM

On the Docker VM, create the Grafana compose project using the existing Docker network
used by Nginx Proxy Manager:

```yaml
services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    networks:
      - wwwarea
    ports:
      - "3003:3000"
    volumes:
      - ./grafana:/var/lib/grafana

networks:
  wwwarea:
    external: true
```

Start Grafana and open its web interface through the configured reverse-proxy hostname:

```bash
docker compose up -d
docker compose logs --tail=50 grafana
```

Add VictoriaMetrics as a Prometheus-compatible Grafana data source using:

```text
http://<observer-management-ip>:8428
```

## Validation

- Confirm every target responds on `/metrics` from observer.
- Open VictoriaMetrics and verify targets are being scraped.
- Open Grafana and verify the data source reports success.
- Confirm dashboards show OPNsense, TrueNAS, Proxmox, Docker VM, and cAdvisor data.
- Confirm the reverse-proxy URL works from the Fedora workstation and permitted remote client.

## Recovery

If a target fails, test network reachability and the target's node-exporter service before
changing VictoriaMetrics. If Grafana fails, inspect container logs and verify permissions
on the `grafana` data directory. Restore persistent data from backup before recreating a
container with destructive volume changes.
