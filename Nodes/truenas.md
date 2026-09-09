# Node Profile — Joe

**Hardware:** Lenovo ThinkCentre M715q Gen2 
**Upgrades:** 1tb Sata SSD, 256gb NVMe, 16gb RAM 
**Role:** NAS  
**Version:** TrueNAS Scale v25.10.4  
**Network:** VLAN11 management → 10.1.11.7  

---

## Services on Joe
NFS and SMB shares. SMB data is at /mnt/TANK/NASgul; NFS data is at /mnt/TANK/nfs.

**Key configurations:** 

---

## Purpose
This node provides network-attached storage for personal devices, as well as backups for my  
containers, configurations, documentation, and more.

---

## Future Improvements
Snapshots and a more robust backup solution than rsync scripts on separate hosts

---

![TrueNAS Console](images/truenas.png)