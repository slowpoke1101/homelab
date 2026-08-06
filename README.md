	The purpose of this homelab is to learn, to apply new concepts/functions/technologies to a  
	hands-on environment as I learn about them. This iteration was to expand what i had built,  
	with the raspberry pi 4 and gli.net flint router, in to purpose built network devices.  
	I wanted a router, a switch, a nas, and a compute node so i purchased a nuc7 mini pc, a  
	managed switch, and two thinkcenter tiny PCs to add to my existing raspberry pi4 and  
	gli.net flint1 and flint2 routers. With this new hardware I would be able to finally  
	configure all the things i've been wanting to configure, as well as further my interests  
	in virtualization, networking and linux.

	My plan was to run a firewall baremetal on the nuc, configure access and trunked ports on the  
	switch, run a hypervisor(proxmox) on one thinkcenter, a NAS OS bare-metal on the other  
	thinkcenter, and use both my flint routers in AP mode as wireless access points. I decided  
	to run opnsense on the nuc and got a usb NIC to use as it's LAN port. I decided to run  
	trueNAS as my nas os. I wanted create real vlans and segmented networks: home/trusted, iot,  
	production, management, and sandbox(maybe a dmz/honeypot in the future). We recently moved  
	to a new condo and the building manages the internet connection so we no longer have a public  
	IP assigned to our unit. I wanted a solution for remote access to my self-hosted apps and  
	services without a public ip since i couldnt run wireguard anymore. I wanted to implement a  
	backup strategy, monitoring, and logging.
	
	I was able to do all of this and more! This past couple of months have been very fun and  
	exciting and there's still so much more to do! the documentation of what i've done and will  
	do are in this repository. Lastly, im new to github and markdown so please excuse the crude  
	formatting
