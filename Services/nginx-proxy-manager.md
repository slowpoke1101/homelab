## nginx proxy manager <img src="images/npm.png" alt="png icon" width="24" height="24">

- reverse proxy server terminating https connections for my web apps and management ui's
- running on docker VM(netopia)
- exposed through nginx proxy manager
- accessible from LONS and izzy only, local or remote(tailscale)
- selfsigned wildcard cert for self-hosted apps
- access list restricted to phone, workstation and management hosts
![webapp route](images/webapp-route.png)
![npm1 dashboard](images/npm1.png)
![npm2 dashboard](images/npm2.png)