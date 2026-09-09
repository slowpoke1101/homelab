## Description
installing Grafana and VictoriaMetrics, a metrics monitoring suite, on Docker

## Purpose
Full observability of node metrics on a centralized dashboard. In the current layout, VictoriaMetrics runs on observer and Grafana runs on netopia.

##

create folder and yaml files
```
sudo mkdir -p /srv/observ/victoriametrics
sudo chown -R /srv/observ/victoriametrics
cd /srv/observ/victoriametrics
sudo vi prometheus.yml
```

paste in
```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'netopia'
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
      - targets: ['10.4.4.1:9100']
```
this scrapes the nodes at their exposed IP:Port for metrics

save & quit  
then create compose yaml
```
sudo vi docker-compose.yml
```

paste in
```
services:
  victoriametrics:
    image: victoriametrics/victoria-metrics:latest
    container_name: victoriametrics
    restart: unless-stopped
    networks:
      - electopia
    ports:
      - "8428:8428"
    volumes:
      - ./data:/victoria-metrics-data
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    command:
      - "--promscrape.config=/etc/prometheus/prometheus.yml"

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

bring the container up and then down so that the grafana database is created
```
docker compose up -d
docker compose down
```

give grafana permissions to the DB
```
sudo chown -R 472:472 /srv/observ/victoriametrics/grafana
```

and finally bring the container up cleanly
docker compose up -d