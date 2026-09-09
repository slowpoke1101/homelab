# Deploy `backup-node`

## Purpose

Deploy a Raspberry Pi 4 with a USB-connected SATA drive as the dedicated local backup node.

## Current Role

`backup-node` is connected to the management VLAN and receives rsync backups from TrueNAS.
The backup schedule is twice daily. Keep this node dedicated to backup and recovery tasks.

## Prerequisites

- Raspberry Pi 4 and USB-to-SATA adapter.
- Storage device with a tested filesystem.
- Raspberry Pi OS installation media.
- Network access to the management VLAN.
- TrueNAS SMB/NFS share details and an account with only the required permissions.
- A documented source, destination, and retention policy before enabling automation.

## Deployment

### 1. Install and update the operating system

Flash Raspberry Pi OS to the SATA boot drive, boot the Pi, apply updates, and configure a
unique hostname and administrative account. Disable unused services and radios only after
confirming they are not required for recovery access.

### 2. Connect storage and verify mounts

Create the local backup directory and add the TrueNAS mount using the dedicated SMB
credentials procedure in [Secure SMB Credentials](smbcred-fstab.md). Use `nofail` and
`x-systemd.automount` where appropriate so a TrueNAS outage does not prevent boot.

```bash
sudo mkdir -p /srv/backup
sudo mount -a
findmnt
```

### 3. Create and test the backup job

Create an rsync script with the confirmed TrueNAS source and local destination. Use
`--dry-run` before enabling deletion or scheduled execution. Log output to a file that can
be reviewed after each run.

```bash
sudo rsync -avh --dry-run /path/to/truenas-share/ /srv/backup/
```

The exact source and destination paths must be filled in from the live configuration; do
not infer them from an old scratch note.

### 4. Schedule the job

Schedule the tested script twice daily using cron or a systemd timer. Ensure the job does
not start a second copy while an earlier run is still active.

### 5. Harden the node

- Use key-based SSH if remote administration is required.
- Disable password SSH authentication after testing keys.
- Restrict firewall access to the management network.
- Keep the OS and rsync packages updated.
- Protect backup storage permissions from non-administrative users.

## Validation

- Confirm the TrueNAS share mounts after reboot.
- Run a dry-run and then a small real backup.
- Confirm the log records success and failure states.
- Delete or modify a test file at the source and verify the expected backup behavior.
- Perform a restore test to a temporary directory.
- Confirm two scheduled runs do not overlap.

## Recovery

If a backup fails, preserve the log, verify network reachability to TrueNAS, check the mount
with `findmnt`, and run rsync manually without `--delete` until the cause is understood.
Never treat a successful rsync exit code as proof that a restore has been tested.
