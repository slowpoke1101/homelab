# Node Profile — Tai

**Hardware:** NETGEAR GS308EP managed switch with PoE+  
**Role:** Network switch  
**Version:** Managed switch firmware / v4 notes  
**Network:** VLAN 1 native LAN → 192.168.33.2 (web UI)

---

## Key configurations
- Port 1: TrueNAS on VLAN 11
- Port 2: Proxmox on VLAN 11 with tagged VLANs 1, 4, 50, 88, and 99
- Port 3: backup-node on VLAN 11
- Port 6: observer on VLAN 11
- Port 7: OPNsense LAN with VLAN 1 native and tagged VLANs 4, 11, 50, 88, and 99
- Port 8: Flint2 on VLAN 1 native with VLANs 88 and 99 tagged

![switch ports](images/switchports.png)

---

## Purpose
This node carries the VLANs created on Mimi and provides access and trunk ports for devices as needed. It also powers the Pi hosts through their PoE+ hats, which is a nice improvement over running separate power bricks everywhere.

---

## Future Improvements
- More runbook-style documentation for deployment and switching changes
- Better labeling and port mapping in the documentation
- A more formal inventory of port usage as the lab expands

---

![switch ss](images/switch1.png)
![switch ss2](images/switch2.png)