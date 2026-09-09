# Node Profile — Sora

**Hardware:** Lenovo ThinkCentre M75q Gen2  
**Upgrades:** 500 GB NVMe, 48 GB 3200 MHz RAM  
**Role:** Proxmox Node  
**Version:** PVE v8.2.1  
**Network:** VLAN11 management (Tai port 2; VLANs 1, 4, 50, 88, and 99 tagged) → 10.1.11.6  

---

## Virtual Machines
- netopia (Docker VM)
- bot1
- bot2
- gui1
- win1

---

## Services on netopia
AdGuard, Gitea, Immich, LibreSpeed, iPerf3, Memos, Nginx Proxy Manager, StirlingPDF,  
Uptime Kuma, Vaultwarden, Grafana, cAdvisor, and Node Exporter

**Key configurations:** 

---

## Purpose
This node exists because early on I found an affinity for virtualization but was limited by hardware.

---

## Future Improvements
Sandbox pentesting, LXC containers, high availability, clustering.

---

![Sora VM Console](images/sora_console.png)
