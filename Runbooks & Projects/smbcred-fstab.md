## Description
Create a secure credential file for smb mounts in /etc/fstab

## Purpose
my /etc/fstab mounts for smb held my NAS user password in plaintext, sketchy
this is more secure


create the credential file
```
sudo nano /root/.smbcred
```
##

inside fill in
```
username=YOUR_TRUENAS_USERNAME
password=YOUR_TRUENAS_PASSWORD
```
##
  
Lock down permissions to root only
```
sudo chmod 600 /root/.smbcred
sudo chown root:root /root/.smbcred
```
##
  
replace username=xxxxxx,password=xxxxxxx, with credentials=/root/.smbcred
```
//NAS/share /local/share cifs credentials=/root/.smbcred,iocharset=utf8,uid=1000,gid=1000,nofail,x-systemd.automount 0 0
```