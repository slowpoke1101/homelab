## Description
Installing node-exporter on TrueNAS node and adding entry to prometheus.yml  

## Purpose
To add node metrics for TrueNas node to Grafana

## Deployment

ON TrueNAS NODE:  
login to truenas web ui as an admin the go to System > Shell

# download node exporter(replace * with version number)
```
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-*.linux-amd64.tar.gz
```

# extract and clean up
```
tar xvf node_exporter-*.tar.gz
sudo rm node_exporter-*.tar.gz
```

# move into zfs pool folder
```
sudo mv node_exporter*/node_exporter /mnt/TANK/<dataset>
```

# Back in the TrueNAS web ui

go to System > settings > advanced settings > init/shutdown scripts
click add
>Description: Node Exporter
>Type: Command
>Command:
>nohup /mnt/TANK/<dataset>/node_exporter-1.12.1.linux-amd64/node_exporter > /dev/null 2>&1 &
>When: Post Init
>Enabled: check yes
>Timeout:10

# check at
http://10.1.11.7:9100/metrics

ON OBSERVER (metrics collector)  
```
ssh banana@<observer-address> #enter password
```

# go to container folder and edit prometheus.yml to add new node exporter entry

```
cd /srv/observ/victoriametrics
sudo vi prometheus.yml
```

# paste at the bottom
```
  - job_name: 'truenas-node'
    static_configs:
      - targets: ['10.1.11.7:9100']

```

# restart VictoriaMetrics so it reloads the scrape configuration
```
docker compose restart victoriametrics
```

# check https://grafana.docker-vm.hub

Dashboards > Node Exporter > Node(dropdown); select truenas-node