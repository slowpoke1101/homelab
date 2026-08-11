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

# move into binary folder
```
sudo mv node_exporter*/node_exporter /usr/local/bin/
```

# paste in text editor
```
sudo vi /etc/systemd/system/node_exporter.service
```

```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=root
ExecStart=/usr/local/bin/node_exporter
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

# restart daemons and enable the new service and check status
```
sudo systemctl daemon-reload
sudo systemctl enable --now node_exporter
sudo systemctl status node_exporter
```

##

ON DOCKER VM(netopia)  
```
ssh banana@netopia.hub #enter password
```

# go to container folder and edit prometheus.yml to add new node exporter entry

```
cd /srv/sata/monitor/victoriametrics
sudo vi prometheus.yml
```

# paste at the bottom
```
  - job_name: 'truenas-node'
    static_configs:
      - targets: ['10.1.11.x:9100']

```

# restart the containers
```
cd ..
cd node-exporter
docker compose restart
cd ..
cd victoriametrics
docker compose restart
```

# check https://grafana.netopia.hub

Dashboards > Node Exporter > Node(dropdown); select truenas-node