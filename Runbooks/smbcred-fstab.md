# Secure SMB Credentials For `/etc/fstab`

## Purpose

Keep TrueNAS SMB credentials out of the `/etc/fstab` command line while allowing a Linux
host to mount a share automatically.

## Prerequisites

- A working TrueNAS SMB share.
- The share hostname or IP address and share name.
- `cifs-utils` installed.

```bash
sudo apt update
sudo apt install cifs-utils
```

## Purpose
## Procedure

Create the credential file as root:
```
sudo nano /root/.smbcred
```
##

Add the SMB account details:
```
username=YOUR_TRUENAS_USERNAME
password=YOUR_TRUENAS_PASSWORD
```
##
  
Lock down permissions so only root can read the file:
```
sudo chmod 600 /root/.smbcred
sudo chown root:root /root/.smbcred
```
##
  
Add a mount entry to `/etc/fstab`. Replace the placeholders with your actual share and
mount paths; do not commit the credential file to the repository.
```
//NAS/share /local/share cifs credentials=/root/.smbcred,iocharset=utf8,uid=1000,gid=1000,nofail,x-systemd.automount 0 0
```

## Validation

```bash
sudo mount -a
findmnt /local/share
ls -la /local/share
```

`nofail` and `x-systemd.automount` reduce boot impact when TrueNAS is unavailable, but the
mount should still be tested after every change to the share or credentials.