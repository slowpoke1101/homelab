# Homelab Changelog

This file records meaningful changes to the homelab's hardware, network, security controls,
services, storage, backups, and monitoring. Entries are listed newest first when the date is
known.

Routine administration and one-off troubleshooting do not need an entry. Record changes that
alter architecture, availability, security, recovery, or service behavior.

---

## Historical Changes

## 2026-07-03

### Added

- Added a second Raspberry Pi 4 to the homelab.
- Added PoE+ hats and a NETGEAR GS308EP PoE+ switch to power both Raspberry Pis.

### Changed

- Dedicated one Raspberry Pi as `backup-node` for TrueNAS backups.
- Moved metrics and log collection from the Docker VM to `observer`.
- Established `observer` as the host for VictoriaMetrics, Loki, and Promtail.

### Documentation Impact

- Architecture documentation now identifies `backup-node` as the backup system and `observer`
  as the metrics and log collector.
- The hardware and network topology documentation reflects the PoE+ switch and both Raspberry Pis.

---

## 2026-09-27 - Migrate TrueNAS to Proxmox Virtual Machine

### Changed

- Converted TrueNAS from bare-metal deployment on Joe to a virtual machine.
- Added raw disk mapping to present storage disks directly to the VM.
- Consolidated storage and virtualization workloads onto Proxmox infrastructure.

### Reason

- Replaced the Lenovo M715q with M75q for testing and lab workloads.
- Increase hardware utilization and flexibility.
- Simplify future expansion of the virtualization environment.

### Impact

- Temporary storage downtime occurred during migration.
- Existing SMB shares and datasets remained unchanged after cutover.
- Storage services are now dependent on the Proxmox host.

### Validation

- Verified SMB shares were accessible from Windows and Linux clients.
- Confirmed dataset integrity.
- Confirmed backup jobs completed successfully.

### Documentation

- Updated xxx.md
- Updated xxx.md
- Updated xxx.md

---

## Entry Template

Copy this section for each future meaningful change and place the newest entry above older entries.

```markdown
## YYYY-MM-DD - Short change title

### Changed

- What changed?

### Reason

- Why was the change made?

### Impact

- What systems, users, networks, or services were affected?

### Validation

- What was tested or confirmed after the change?

### Documentation

- Which architecture page, runbook, service page, or troubleshooting note was updated?
```
