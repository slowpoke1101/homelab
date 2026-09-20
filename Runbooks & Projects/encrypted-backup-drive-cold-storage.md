# Encrypted Backup Drive For Cold Storage

## Purpose

Create an encrypted removable backup drive for offline cold storage. The drive uses LUKS2
for full-disk encryption and ext4 for the filesystem inside the unlocked volume.

This is an additional backup copy. It does not replace the scheduled rsync backups from
Joe to TK or restore testing.

## Prerequisites

- A Linux host such as Izzy or TK with `cryptsetup` installed.
- The removable drive connected directly to the host.
- A confirmed source backup, such as the required data from Joe.
- A strong passphrase stored in an approved password manager or other secure location.
- A separate, secure place to store the drive when it is not being updated.

Install the required tools on Debian or Ubuntu if needed:

```bash
sudo apt update
sudo apt install cryptsetup rsync
```

## Expected Impact

The drive will be erased. Confirm the device identifier carefully before running
`wipefs` or `cryptsetup luksFormat`. Do not use a device that contains the operating
system or any data that has not been backed up.

## Procedure

### 1. Identify the removable drive

Connect the drive and inspect its device name, size, and model:

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL,SERIAL
```

Prefer the stable path under `/dev/disk/by-id/` instead of relying on `/dev/sdb`, which can
change between boots:

```bash
ls -l /dev/disk/by-id/
```

Set `DISK` to the confirmed whole-disk path. It must refer to a disk, not a partition such
as `${DISK}1`.

```bash
export DISK=/dev/disk/by-id/REPLACE_WITH_THE_CORRECT_DISK
lsblk "$DISK"
```

### 2. Remove old filesystem metadata

This step is optional if the drive is already blank, but it removes old partition and
filesystem signatures before creating the encrypted volume:

```bash
sudo wipefs -a "$DISK"
```

Run `lsblk` again and confirm that the expected drive has no required partitions or mounts.

### 3. Create the LUKS encrypted volume

Create a LUKS2 container on the whole drive. Confirm the warning and enter the passphrase
when prompted:

```bash
sudo cryptsetup luksFormat --type luks2 "$DISK"
```

Open the encrypted container using the mapping name `coldbackup`:

```bash
sudo cryptsetup open "$DISK" coldbackup
```

The unlocked device is available at `/dev/mapper/coldbackup`.

### 4. Create the filesystem and mount it

Create an ext4 filesystem and label it for easier identification:

```bash
sudo mkfs.ext4 -L cold-backup /dev/mapper/coldbackup
sudo mkdir -p /mnt/cold-backup
sudo mount /dev/mapper/coldbackup /mnt/cold-backup
```

Verify that the expected device is mounted:

```bash
findmnt /mnt/cold-backup
df -h /mnt/cold-backup
```

### 5. Copy the backup data

Replace the source with the confirmed backup location. The trailing slash means rsync
copies the contents of the source directory into the mounted destination:

```bash
sudo rsync -avh --info=progress2 /path/to/backup-source/ /mnt/cold-backup/
```

Review the rsync output for errors. For a second pass, use a dry run to check whether files
would still be copied:

```bash
sudo rsync -avhn --delete /path/to/backup-source/ /mnt/cold-backup/
```

Do not add `--delete` to the real copy unless deleting files from the cold-storage copy is
intentional and the source has been verified.

### 6. Unmount and lock the drive

Always lock the volume before disconnecting the drive:

```bash
sudo umount /mnt/cold-backup
sudo cryptsetup close coldbackup
```

Confirm that the mapping is closed:

```bash
lsblk
```

Disconnect the drive and store it offline in a secure location.

## Validation

- Confirm the source and destination paths before copying.
- Confirm `findmnt` shows the expected encrypted mapping and mount path.
- Review the rsync output for errors and confirm the dry run reports no unexpected copies.
- Reconnect the drive, unlock it, and verify that representative files open correctly.
- Perform a test restore to a temporary directory rather than restoring over live data.
- Confirm the drive is unmounted and the `coldbackup` mapping is closed before storage.

## Recovery And Limitations

To access the drive again, identify it, unlock the LUKS container, and mount the mapping:

```bash
sudo cryptsetup open /dev/disk/by-id/REPLACE_WITH_THE_CORRECT_DISK coldbackup
sudo mount /dev/mapper/coldbackup /mnt/cold-backup
```

When finished, run the unmount and close commands from step 6.

If the LUKS passphrase is lost, the encrypted data cannot be recovered through this
procedure. Keep the passphrase separate from the drive, and maintain another working
backup before making changes to the cold-storage copy.

## Related Documentation

- [backup-node deployment](rpi4-backupnode-DEPLOYMENT.md)
- [Security Architecture](../Architecture%20Overview/security-architecture.md)