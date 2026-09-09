after building the mini rack, my vms on sandbox50 network were getting apipa addresses.
for context, the VMs run on a Proxmox host whose switch port is tagged with 1, 4, 50, 88, and 99.
all networks other than 50 were working fine, my production services and apps were all up and reachable which meant that the vlan paths and interface configs were working, but why did only one network break? after pinging and testing network paths/connections i was vexed.  
i recently updated opensense AND proxmox and checked to make sure proxmox vlan-aware was still active, i unchecked it, tested, and checked it.  
i finally thought, it must be somewhere where that could mention the vlan tags individually. then i realized that when I built the rack I switched OPNsense's switch port to a previous access port for VLAN 50. When I configured it as a trunk for all my VLANs, I changed the PVID (native VLAN) to VLAN 1 untagged but left VLAN 50 untagged as it had been on the old access port. There were two untagged VLANs, so traffic to VLAN 50 was silently dropped.
lesson learned: remember to double check tagged and untagged vlans/ports after making any switch port change.
