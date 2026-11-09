# TrueNAS SCALE VM Shutdown Hang in Proxmox

## Summary

A TrueNAS SCALE VM consistently failed to shut down when hosted on Proxmox. The issue occurred regardless of whether shutdown was initiated from the TrueNAS web interface or from the Proxmox UI.

The VM remained operational during normal use and all storage-related functionality appeared healthy. The issue was ultimately resolved by changing the VM display adapter from SPICE/QXL to the Default display type.

**Resolution Confidence:** High

**Root Cause Confidence:** Low (unconfirmed)

---

## Environment

### Hypervisor

- Proxmox VE
- UEFI (OVMF)
- Q35 machine type

### TrueNAS VM

- TrueNAS SCALE
- OS disk stored on Proxmox local-zfs
- Data disk presented through raw disk mapping

Example configuration:

```bash
qm set  -scsi1 /dev/disk/by-id/
```

Storage layout:

```text
Proxmox Host
├── local-zfs
│   └── TrueNAS OS disk
└── Physical SSD
    └── Raw-mapped to TrueNAS VM
```

---

## Symptoms

The VM functioned normally while running:

- TrueNAS web UI accessible
- SMB shares operational
- Datasets accessible
- Applications functioning normally
- Pool imported successfully
- Files could be read and written from client systems

However, shutdown consistently failed.

### Behavior Observed

Shutdown initiated from:

- TrueNAS web UI
- Proxmox VM controls

Result:

- TrueNAS web UI became unavailable
- VM never completed shutdown
- `qm status <VMID>` continued to report:

```text
status: running
```

- VM remained in this state indefinitely (observed 15+ minutes)

The Proxmox console remained on the standard TrueNAS text menu:

```text
1) Configure Network Interfaces
2) Configure Network Settings
...
10) Open Shell
```

No visible shutdown progress messages appeared.

---

## Initial Suspicions

Potential causes investigated conceptually included:

- Raw disk mapping issues
- ZFS pool shutdown problems
- SSD communication issues
- Application/container shutdown hangs
- TrueNAS middleware issues

These became less likely because:

- Pool imports were successful
- SMB shares worked reliably
- Datasets remained accessible
- No storage instability was observed
- Runtime operation was completely normal

---

## Resolution

Changed the VM display adapter from:

```text
SPICE / QXL
```

to:

```text
Default
```

After making this change:

- Shutdown from the TrueNAS web UI succeeded
- Shutdown from Proxmox succeeded
- VM powered off normally
- Issue has not reoccurred

---

## Root Cause

Unknown.

The only configuration change required to resolve the problem was changing the VM display device.

This suggests the issue may have involved:

- SPICE/QXL display emulation
- Console interaction during shutdown
- Guest compatibility with virtual display hardware
- Some other virtualization-related behavior

No direct evidence was collected to confirm any specific explanation.

---

## Lessons Learned

1. Do not assume shutdown issues are storage-related simply because a storage appliance is involved.

2. If runtime storage behavior is healthy, investigate VM hardware configuration before modifying storage settings.

3. Separate observed facts from assumptions when troubleshooting.

4. A documented workaround with verified results is still valuable even when the precise root cause remains unknown.

---

## Final Status

- Issue resolved

- Shutdown functional from TrueNAS web UI

- Shutdown functional from Proxmox

- Storage pool remains healthy

! Root cause not definitively identified
