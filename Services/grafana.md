## grafana <img src="images/grafana.png" alt="png icon" width="24" height="24">

- self-hosted monitoring dashboard 
- running on docker VM(netopia)
- exposed through nginx proxy manager
- accessible from LONS and izzy only, local or remote(tailscale)
- cAdvisor providing container visibility
- node exporter on both the docker VM and proxmox node.
- future updates: add exporters from truenas and opnsense nodes.
                  add log dashboard with syslogs collected by rpi4 node
![webapp route](images/webapp-route.png)
![grafana dashboard](images/grafanadash.png)