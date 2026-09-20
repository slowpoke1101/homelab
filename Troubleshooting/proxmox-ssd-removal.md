# Proxmox Failed After SSD Removal

## Summary

Proxmox became partially unavailable after an unused SATA SSD was removed from the
ThinkCentre. The host responded to ping but its web UI was not reachable.

## Environment

- Proxmox runs on the ThinkCentre M75q Gen2.
- The removed SSD had previously been mounted at `/srv/sata`.
- The Docker VM had previously used the drive.
- The drive was removed after its Proxmox storage entry and VM attachment were removed.

## Symptoms

- The physical SATA SSD was removed from the Proxmox host.
- The host booted far enough to respond to ping.
- The Proxmox web UI was unavailable.

## Investigation

1. Reinstalled the SATA SSD and rebooted the host.
2. Confirmed that Proxmox then booted normally.
3. Checked `/etc/pve/storage.cfg` and found no remaining reference to the SATA storage entry.
4. Checked `/etc/fstab` and found the old mount entry:

   ```text
   /dev/sda1 /srv/sata ext4 defaults 0 2
   ```

## Root Cause

The storage entry had been removed from Proxmox, but the operating-system mount entry
remained in `/etc/fstab`. During startup, the host still attempted to mount the removed
device. Because the entry did not use a failure-tolerant option, the missing drive
interfered with normal startup.

## Resolution

1. Commented out the obsolete `/etc/fstab` entry for `/dev/sda1`.
2. Rebooted Proxmox with the SSD still installed to confirm the mount was no longer required.
3. Removed the physical SSD.
4. Rebooted again and confirmed normal operation.

## Validation

- Proxmox booted successfully without the SATA SSD.
- The host remained reachable.
- The Proxmox web UI became available.
- The Docker VM continued to operate from its remaining storage.

## Prevention

Before removing a physical disk, check all layers that may reference it:

- Proxmox storage configuration in `/etc/pve/storage.cfg`.
- VM or container hardware assignments.
- Operating-system mounts in `/etc/fstab`.
- Systemd mount units and backup scripts.
- Application paths that depend on the disk.

After removing each dependency, reboot or perform a controlled mount test before physically
disconnecting the drive.
