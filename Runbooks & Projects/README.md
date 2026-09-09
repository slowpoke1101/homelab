# Runbooks And Projects

This folder contains repeatable procedures for deploying, configuring, operating, and
recovering homelab systems. The architecture documents describe the intended current state;
these runbooks explain how to create or change it.

## Before Making Changes

1. Confirm the current branch is `biff`.
2. Review the relevant architecture page and current node profile.
3. Record a backup or rollback plan before making a destructive change.
4. Use placeholders for passwords, tokens, certificates, and private keys.
5. Validate the change and update the changelog and architecture documentation when the
   change becomes part of the normal design.

## Runbook Index

### Networking

- [OPNsense installation and initial configuration](DEPLOYMENT-opnsense/DEPLOYMENT.md)
- [Managed switch deployment](DEPLOYMENT-netgearswitch/NetgearSwitch-DEPLOYMENT.md)
- [Flint2 wireless access point deployment](DEPLOYMENT-wirelessaccesspoint/Deployment.md)

### Systems, Storage, And Backups

- [Docker installation](docker-install.md)
- [SMB credentials and fstab](smbcred-fstab.md)
- [backup-node deployment](rpi4-backupnode-DEPLOYMENT.md)

### Monitoring And Logging

- [Metrics stack deployment](DEPLOYMENT-Logs-Monitoring/Grafanana-Victoriametrics-install.md)
- [Syslog forwarding](DEPLOYMENT-Logs-Monitoring/syslog-forwarding-config.md)
- [Logging stack deployment](DEPLOYMENT-Logs-Monitoring/Loki-Promtail-install.md)
- [OPNsense node exporter](DEPLOYMENT-Logs-Monitoring/Node-exporter-INSTALL/node-exporter-opnsense.md)
- [Proxmox node exporter](DEPLOYMENT-Logs-Monitoring/Node-exporter-INSTALL/node-exporter-proxmox.md)
- [TrueNAS node exporter](DEPLOYMENT-Logs-Monitoring/Node-exporter-INSTALL/node-exporter-truenas.md)

## Runbook Standard

A complete runbook should identify its purpose, scope, prerequisites, expected impact,
procedure, validation steps, rollback or recovery path, and related documentation. A
runbook should be reproducible by a future version of you, not just understandable to the
version of you who performed the original work.
