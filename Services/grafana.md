## grafana <img src="images/grafana.png" alt="png icon" width="24" height="24">

- self-hosted monitoring dashboard 
- running on observer, the Raspberry Pi 4 metrics and log collector
- exposed through nginx proxy manager
- accessible from LONS and izzy only, local or remote(tailscale)
- cAdvisor providing netopia container visibility
- node exporter runs on Mimi, Joe, Sora, and netopia
- Loki and Promtail run on observer for centralized log collection
![webapp route](images/webapp-route.png)
![grafana dashboard](images/grafanadash.png)