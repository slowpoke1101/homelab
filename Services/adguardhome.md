## adguard home <img src="images/adguard-home.png" alt="png icon" width="24" height="24">

- Ad blocking has been offloaded to OPNsense so that DNS doesnt break when  
  the VM needs to be shutdown. This is a remnant of the original docker vm
  install and the host's networking stack depends on it. bringing the container
  down breaks DNS for the docker VM(netopia) so I will have to figure out how to
  fix this when i find time
- running on docker VM(netopia)
- exposed through nginx proxy manager
- accessible from LONS and izzy only, local or remote(tailscale)
![webapp route](images/webapp-route.png)