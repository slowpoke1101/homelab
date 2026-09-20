## trueNAS <img src="images/truenas.png" alt="png icon" width="24" height="24">

- NAS operating system running directly on TrueNAS, a ThinkCentre Tiny, providing network storage
- web ui exposed through nginx proxy manager
- web ui accessible from workstation with local connection only
- access to SMB shares allowed only to iOS, Fedora, and backup-node
- access to NFS share allowed only to the Docker VM and backup-node
- SMB data is stored at /mnt/TANK/NASgul
- NFS data is stored at /mnt/TANK/nfs
![truenas route](images/truenas-route.png)
![truenas dashboard](images/truenasdash.png)