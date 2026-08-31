##
first plug flint2 into switchport with pvid1 and tags 88 and 99  
locate flint2's IP address in opnsense web UI to reach gui
navigate to Network > Network Mode > choose access point: flint will now get dhcp from opnsense and flint's ip will change
<img src="images/WAPmode.png" alt="wapmode" width="50%">
navigate to flint ui at new ip address then system > advanced settings> install luci
navigate to the luci ui and log in

##
find the Network tab at the top and choose Interfaces then on the interfaces page find the Devices tab
<img src="images/navigationinterfaces.png" alt="navigation1" width="50%">
<img src="images/navigationdevices.png" alt="navigation2" width="50%">
On the devices tab click add device configuration
<img src="images/adddevice.png" alt="adddevice" width="50%">
for Device Type: VLAN(802.1q) 
<img src="images/adddevice2.png" alt="adddevice2" width="50%">
for VLAN ID: 88(in my case but chose whatever tag you need propagated to WiFi)
<img src="images/adddevice3.png" alt="adddevice3" width="50%">
click save
make sure to click save and apply at the bottom after maky changes

##
navigate back to the Interfaces tab and click add new interface
<img src="images/addinterface.png" alt="addinterface" width="50%">
under Name: IOT99(in my case, im creating interfaces for 88 and 99 so screenshots will reflect both)
Protocol: Unmanaged
<img src="images/addinterface2.png" alt="addinterface2" width="50%">
Device: Sofware VLAN: "br-lan.99"
<img src="images/addinterface3.png" alt="addinterface3" width="50%">
click create interface

##
navigate back to the Devices tab, find "br-lan" and click configure
click the bridge VLAN filtering tab, check "Enable bridge VLAN filtering" and at the bottom add vlans as necessary
in my case my connection from the switch goes in to LAN1 so i left vlan 1 untagged and tagged 88, and 99.
<img src="images/bridgedVLAN.png" alt="VLAN" width="50%">

##
navigate to Network Dropdown at the top then Wireless
<img src="images/navigationwireless.png" alt="navigation3" width="50%">
find the radio that shows 5ghz and click add
<img src="images/addwireless.png" alt="addwireless" width="50%">
at the bottom half of the page input your desired ESSID: IOT99(in my case) 
Network: IOT99 
<img src="images/addwireless2.png" alt="addwireless2" width="50%">
then click the next tab over Wireless Security
choose your encryption type: wpa2-psk (in my case)
input your Key: (wirless password, chose a strong password)
<img src="images/addwireless3.png" alt="addwireless3" width="50%">
click save

