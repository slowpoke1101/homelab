## Description
Deploying a raspberry pi 4 + USB to sata ssd boot drive as an onsite rsync backup node

## Purpose
since i now have 2 rpi4's i'm redeploying my backup node headless, and on a new ssd  
(it was previousyly running from an old laptop hdd). This can also serve as the first  
step in the deployment of my soon to be remote rpi4 backup node that will do the exact  
same thing but over Tailscale from a remote location thus completing my 3-2-1 setup.

##
Step 1 flash the SSD with rpi imager using rufus or balenaetcher then plug it into  
the pi and boot. update and minimalize the os(disable bluetooth, wifi, unused  
services like cups, etc.)

##
step 2 create NFS and SMB mount points in /etc/fstab

##
step 3 create rsync backup script and test

##
step 4 add to cron

##
step 5 harden



