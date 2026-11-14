# Node Profile — backup-node

**Hardware:** Raspberry Pi 4 4GB, POE+ Hat, USB SATA adapter, and external 1TB HDD or SSD for the OS and backup data
**Role:** Backup node 
**OS:** Raspberry Pi OS (Debian 13 Trixie)  
**Network:** Management VLAN 11 → 10.1.11.9

---

## Services
- rsync backups from TrueNAS several times daily
- basic troubleshooting and enumeration tools
- backup scripts for local copies and restore prep

---

## Key configurations
- Runs as a dedicated backup host after the main network roles were filled by the newer hardware.
- Keeps a second copy of key data from TrueNAS instead of relying on a single storage host.
- Lives on the management network so it can receive logs and run scheduled backup jobs.
- Remote Cold Storage backups of this disk are taken once a month until I  
come up with a remote backup solution.

---

## Purpose
This node was originally a bit of an afterthought once the main roles were covered, but it ended up being useful for backups and troubleshooting. It gives me a small dedicated Linux box that can stay on the management VLAN and do support jobs without adding noise to the main infrastructure hosts.

---

## Future Improvements
- More formal backup validation and restore testing
- Better alerting for backup failures

