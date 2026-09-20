# Node Profile — observer

**Hardware:** Raspberry Pi 4  
**Role:** Metrics and log collector  
**OS:** Raspberry Pi OS (GUI)  
**Network:** Management VLAN 11 → 10.1.11.8 or current management assignment defined in the network docs

---

## Services
- VictoriaMetrics for time-series metrics
- Loki for centralized logs
- Promtail / Alloy-style log forwarding
- Docker host for the observability stack
- Host-level monitoring and log collection for the rest of the lab

---

## Key configurations
- Observer is the central place for monitoring and logs in the homelab.
- Node Exporter runs on the main infrastructure hosts and sends metrics to VictoriaMetrics on observer.
- Syslog is forwarded from infrastructure nodes to observer, where Loki and Promtail handle collection.
- Grafana stays on the Docker VM, while observer provides the storage and collection backend for the graphing and logging layers.
- The node remains on the management VLAN so it can collect telemetry without exposing the observability stack unnecessarily.

---

## Purpose
This node exists to give me a centralized monitoring and logging layer. Instead of letting the Docker VM do everything, I split observability onto a dedicated Pi so the metrics and logs have their own place and don’t depend on the application VM being fully healthy.

---

## Future Improvements
- Add better backup and retention planning for the monitoring data
- Document the exact Docker Compose layout and storage directories
- Add more monitoring targets and alert tuning as the lab grows
