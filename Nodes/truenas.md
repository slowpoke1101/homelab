# Node Profile — Joe

**Hardware:** Lenovo ThinkCentre M715q Gen2  
**Upgrades:** 1TB SATA SSD, 256 GB NVMe, 16 GB RAM  
**Role:** NAS  
**Version:** TrueNAS Scale  
**Network:** VLAN 11 management → 10.1.11.7

---

## Services on Joe
- SMB shares for personal file storage and general use
- NFS exports for the Docker VM and other nodes
- Storage for backups, configs, and documentation

The documented storage paths are:
- `/mnt/TANK/NASgul` for SMB storage
- `/mnt/TANK/nfs` for NFS-backed data

---

## Key configurations
- Joe provides the main storage layer for the homelab.
- SMB is used for general file access and NFS is used for backup and application data on the Docker VM.
- Container and configuration backups are written to the NFS share before being copied to backup-node.
- The Docker VM mounts the relevant shares through `/etc/fstab` so services do not have to be rebuilt constantly.

---

## Purpose
This node provides network-attached storage for personal devices and also keeps copies of my containers, configs, and documentation. It is one of the core stability pieces in the homelab.

---

## Future Improvements
- Better snapshots and snapshot retention
- A more robust backup strategy than relying only on rsync scripts
- More documentation around storage layout and restore testing

---

![TrueNAS Console](images/truenas.png)