Install rsyslog on observer (host)
bash

sudo apt update
sudo apt install rsyslog

Configure /etc/rsyslog.d/central.conf:
bash

module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")

$template RemoteLogs,"/var/log/central/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs

Restart:
bash

sudo systemctl restart rsyslog

 Configure nodes to send logs → observer

Each node forwards syslog to backup-node's IP on port 514.

    OPNsense: Logging Targets → backup-node IP → UDP 514

    Proxmox: *.* @BACKUP_NODE_IP:514 in /etc/rsyslog.conf

    TrueNAS: Syslog → backup-node IP

    Docker VM: *.* @BACKUP_NODE_IP:514

    switch (GS308EP): Syslog → backup-node IP

    Flint APs: Syslog → backup-node IP (if supported)


    services:
  victoriametrics:
    image: victoriametrics/victoria-metrics:latest
    container_name: victoriametrics
    restart: unless-stopped
    networks:
      - wwwarea
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

  loki:
    image: grafana/loki:2.9.0
    container_name: loki
    restart: unless-stopped
    networks:
      - wwwarea
    ports:
      - "3100:3100"
    volumes:
      - ./loki:/loki

  alloy:
    image: grafana/alloy:latest
    container_name: alloy
    restart: unless-stopped
    networks:
      - wwwarea
    volumes:
      - /var/log/central:/var/log/central:ro
      - ./config.alloy:/etc/alloy/config.alloy:ro
    depends_on:
      - loki
      - victoriametrics

networks:
  wwwarea:
    external: true


sudo chmod 777 /srv/observ/victoriametrics/loki










logging {
  level = "info"
}

# Send logs to Loki
loki.write "local" {
  endpoint {
    url = "http://loki:3100/loki/api/v1/push"
  }
}

# Read syslog files collected by rsyslog
filelog "syslog" {
  targets = ["/var/log/central/*/*.log"]
  labels = {
    job = "syslog"
    host = "observer"
  }
}


