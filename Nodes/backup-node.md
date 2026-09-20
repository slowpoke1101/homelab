# Node Profile — backup-node

**Hardware:** Raspberry Pi 4 (4GB/8GB depending on the build note), USB SATA adapter, and external 1TB HDD or SSD for the OS and backup data
**Role:** Backup node, emergency access console, rsyslog receiver, and ancillary monitoring host  
**OS:** Raspberry Pi OS (Debian 13 Trixie)  
**Network:** Management VLAN 11 → 10.1.11.9

---

## Services
- rsync backups from TrueNAS shares twice daily
- rsyslog / syslog collection
- SNMP-related monitoring tasks
- basic troubleshooting and enumeration tools
- backup scripts for local copies and restore prep

---

## Key configurations
- Runs as a dedicated backup host after the main network roles were filled by the newer hardware.
- Keeps a second copy of key data from TrueNAS instead of relying on a single storage host.
- Lives on the management network so it can receive logs and run scheduled backup jobs.
- Acts as an emergency console when I need a lightweight Linux box on the network.

---

## Purpose
This node was originally a bit of an afterthought once the main roles were covered, but it ended up being useful for backups, logging, and troubleshooting. It gives me a small dedicated Linux box that can stay on the management VLAN and do support jobs without adding noise to the main infrastructure hosts.

---

## Future Improvements
- More formal backup validation and restore testing
- Better alerting for backup failures
- Possibly adding a second Pi or a more permanent backup workflow to match the rest of the homelab

