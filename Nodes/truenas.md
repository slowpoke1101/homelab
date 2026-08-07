# Node Profile — Joe

**Hardware:** Lenovo ThinkCentre M715q Gen2 
**Upgrades:** 1tb Sata SSD, 256gb NVMe, 16gb RAM 
**Role:** NAS  
**Version:** TrueNAS Scale v25.10.4  
**Network:** VLAN11 → 10.1.11.7  

---

## Services on Joe
NFS, SMB shares at /mnt/TANK/NASgul

**Key configurations:** 

---

## Purpose
This node provides network attatched storage for my personal devices, as well as backups for my  
containers, configurations, documentation, and more.

---

## Future Improvements
Snapshots, a different back up solution than rsync scripts on separate hosts

---

![TrueNAS Console](images/truenas.png)