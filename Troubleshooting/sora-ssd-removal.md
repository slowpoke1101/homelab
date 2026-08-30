Im getting another rpi4 and wanted to remove the unused sata ssd from node sora(proxmox). i thought it would be as simple as  removing the hardware device from the only VM using it(netopia) as well as removing the local sata entry from Datacenter in  
the Proxmox web UI. However, after doing this and removing the physical sata drive from the thinkcenter, the PVE failed.  
i was able to ping the device but unable to reach the web UI. my guess was that something in the proxmox virtual environment  
was expecting the sata mount, and startup was hanging because the sata was removed.  
So i put the sata ssd back in the thinkcenter and proxmox booted up perfectly. i checked /etc/pve/storage.cfg to find no reference to the sata drive( so removing the entry from Datacenter worked), then i checked /etc/fstab to find and remember  
that i had created a mount entry for the sata drive when i first installed it into the thinkcenter. i commented out the line  
/dev/sda1 /srv/sata ext4 defaults 0 2  
rebooted to check that proxmox would boot correctly without looking for the mount
bam it worked, i then took the drive out and everything all good
lesson learned: remember to check fstab when removing a physical drive