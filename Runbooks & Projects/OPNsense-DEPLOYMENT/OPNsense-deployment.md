## Description
Installing OPNsense; an open-source firewall/router operating system that you can  
install and use on many different systems.
This tutorial is for installing on a NUC7i5BNK, creating interfaces and assigning vlans, configuring dns, dhcp, basic firewall rules for  
each interface, and basic hardening.

## Purpose
I never documented my first deployment of OPNsense(it's a scratch txt file), also this most recent version update from 26.1 to 26.7 has changed the   firewall management and requires rewriting all rules so i figured I would start from scratch, learn the new firewall rule  system, and properly document the deployment of my homelab firewall/router.

## HEADS-UP
this tutorial is the first part of setting up a vlan segmented network. you will need  
a managed switch as well. you can complete these steps first while the switch has the default configurations. the switch configuration comes after unless you already know how the switch is configured and are planning accordingly

##
INSTALL
first steps are done from the console so i was unable to take screenshots
step 1 download img and flash to usb drive
download from https://opnsense.org/download/ choose amd architecture, image type vga, mirror location opnsense
flash to usb drive using rufus windows, balenaetcher linux, the application is very straightforward, choose the file you downloaded from opnsense as the img, and then choose the usb drive and install.
step 2 boot nuc7 from USB(f10 for boot options) and install. opnsense installer will start automatically
the login screen will display the ip address that the web ui will be reachable at, in my case 192.168.1.1/24
it will also display the names of your NICs and will auto assign WAN and LAN
in my case em0-LAN(native NIC) ue0-WAN(USB NIC)
LAN is the nuc's native NIC so i connect my switch to that port, then connect my  
laptop to the switch and will be able to reach the OPNsense web UI at the IP address  
displayed on the console after install.
##
CONFIGURE
step 3 create vlans (repeat for as many vlans as you need)
login to the web ui navigate to Interfaces > Devices > VLAN  
and click the + sign  
<img src="images/navigationtovlans.png" width="50%">  
Device: must start with 'vlan0' i like to match everything i can to the tag for simplicity  
Parent: em0 (choose the NIC that is associated with LAN)
VLAN tag: 99 (this is for my iot but you can put whatever your tags are)
this will create subinterfaces on the parent NIC for each VLAN  
<img src="images/addvlan.png" width="50%">

step 4 assign vlans to interfaces
navigate back to Interfaces > Assignments and click +
the Device dropdown will show the vlans you just created.  
choose each one and name it in the Description field.  
this will create network interfaces for each assigned VLAN
after this you're created interfaces will show up under Interfaces on the left menu  
<img src="images/addassignment.png" width="50%">

step 5 configure interfaces
navigate back to Interfaces and select one of your created interfaces(IOT99 in my case)  
and click enable (more options will appear below)  
<img src="images/interfaces1.png" width="50%">
IPv4 Configuration Type: Static IPv4
more options will appear below
<img src="images/interfaces2.png" width="50%">
IPv4 address: 10.1.99.1  
This is where you will assign the ip address and subnet mask of your firewall for this interface
click save  
<img src="images/interfaces3.png" width="50%">

step 6 configure dhcp pools
navigate to Services > Dnsmasq DNS & DHCP > DHCP ranges and click +
Interface: IOT99 (choose the interfaces you need to configure dhcp pools on)
Start address: 20 (whatever you like within your subnet range, mine is /26)
End address: 62
Subnet mask: 255.255.255.224 (this gives me 62 usable IP addresses)
Lease time: 43200(in seconds)
click save  
<img src="images/dhcprange.png" width="50%">

step 7 basic firewall rules
Originally OPNsense came with Default Allow firewall rules and during the first  
install i built my firewall rules around this. Later i migrated to a Default Deny  
base, so during this new install I applied only the following 3 rules as a   
baseline before i fine tuned everything.
1- allow dns to the firewall
2- allow forward to all except private ip space
3- deny all
First you need to create an Alias for all private ip ranges that you will   reference in a rule later
Navigate to Firewall > Aliases and click +
Name: RFC1918 
Content: 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
click save
<img src="images/aliasesrfc1918.png" width="50%">
Navigate back to Firewall > Rules find the interface you want to configure in the drop-down and click +
<img src="images/firewall1.png" width="50%">
these are the rules in order from top to bottom( since the firewall will consult  
each rule in this order)
rule1
Action: Pass
Protocol: TCP/UDP
Source: IOT99 network
Destination: This Firewall
Destination Port: 53 (DNS)
rule2
Action: Pass
Source: IOT99 network
Invert Destination: CHECKMARK THE BOX (this will set the destination to everyting BUT the selected Destination below, i.e. allow everything EXCEPT private address space)  
Destination: RFC1918 (the alias you created earlier)
rule3
Action: Block
Source: IOT99 network
These rules will allow dns and outgoing internet connections but deny any traffic  
to subnets behind your OPNsense firewall. This is a good baseline to fine tune  
your firewall rules as needed.
<img src="images/firewall2.png" width="50%">

step 8 basic hardening
always good to double check block bogons and block private networks is  
enables on WAN
navigate to Interfaces > [WAN]
<img src="images/blockbogons.png" width="50%">
For security I have SSH disabled on my Router, Wireless AP, Proxmox node, and  TrueNAS node as I have easy console access to them.
Navigate to System > Settings > Administration scroll down to Secure Shell  
UNCHECK Enable Secure Shell  
I noticed that by default ssh listens on ALL interfaces including WAN  
It might be a good idea to specifically select the Interfaces you want SSH  
to be listening on unless you know what you're doing
<img src="images/sshharden.png" width="50%">
I found the same thing for Unbound DNS that it also listens on WAN  
so you might want to specifically select the Interfaces that you want Unbound  
listening on as well
<img src="images/dnsharden.png" width="50%">

##

With these steps in place, the next thing to do is to configure the managed switch
that will propagate the vlans/network interfaces that were just created. and after  
that, a wireless access point for WiFi access.


step 6 set dhcp reservations
step 9 create my network specific firewall rules
step 10 enable IDS and create dns overrides for services/apps/nodes
step 11 reconfigure tailscale for remote access













