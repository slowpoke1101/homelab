## Description
Installing OPNsense on a NUC7i5BNK, creating interfaces and assigning vlans, configuring dns, dhcp, and firewall rules for  
each interface, and basic hardening.

## Purpose
I never documented my first deployment of OPNsense(it's a scratch txt file), also this most recent version update from 26.1 to 26.7 has changed the   firewall management and requires rewriting all rules so i figured I would start from scratch, learn the new firewall rule  system, and properly document the deployment of my homelab firewall/router/gateway.

##
INSTALL
step 1 download img and flash to usb drive
download from https://opnsense.org/download/ choose amd architecture, image type vga, mirror location opnsense
flash to usb drive using rufus windows, balenaetcher linux, the application is very straightforward, choose the file you downloaded from opnsense as the img, and then choose the usb drive and install.
step 2 boot nuc7 from USB and install opnsense

CONFIGURE VLANS AND INTERFACES, assign dns, dhcp, and basic firewall rules, 
step 0 swap WAN and LAN interfaces
step 1 create vlans
step 2 create interfaces and assign vlans to them
step 3 set dhcp pools and basic firewall rules
step 4 set dhcp reservations
step 5 harden settings(system - settings - administration) ssh/webUI access, make sure block bogons/privatenetworks is on,
step 6 create my network specific firewall rules
step 7 enable IDS and create dns overrides for services/apps/nodes
step 8 reconfigure tailscale for remote access








# example image reference<img src="images/rack1.JPG" alt="rack1" width="50%">








Step 1: install docker
```
#install dependencies
sudo apt update
sudo apt install ca-certificates curl gnupg
```

Step 2: add dockers official gpg key
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
