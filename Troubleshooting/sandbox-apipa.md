# Sandbox VMs Received APIPA Addresses

## Summary

Sandbox VMs on VLAN 50 received automatic private IP addresses (APIPA) instead of
DHCP leases. Other VLANs continued to work normally.

## Environment

- Proxmox hosts the affected sandbox VMs.
- The Proxmox switch port carries VLANs 1, 4, 50, 88, and 99.
- VLAN 50 is the isolated sandbox network.
- OPNsense provides routing and DHCP.

## Symptoms

- VMs on VLAN 50 received APIPA addresses.
- Production services and other VLANs remained reachable.
- Proxmox VLAN awareness was enabled and appeared correct.

## Investigation

1. Confirmed that the problem was isolated to VLAN 50.
2. Tested connectivity and network paths from the affected VMs.
3. Checked OPNsense and Proxmox VLAN configuration.
4. Reviewed the individual tagged and untagged VLAN assignments on the switch ports.

## Root Cause

The OPNsense switch port had previously been configured as an access port for VLAN 50.
After changing it to a trunk, VLAN 1 was correctly made the untagged/native VLAN, but
VLAN 50 was accidentally left untagged as well. A trunk port must not have two untagged
VLANs. The invalid tagging caused VLAN 50 traffic to be dropped, preventing DHCP from
reaching the sandbox VMs.

## Resolution

Corrected the OPNsense switch port so that:

- VLAN 1 is the only untagged/native VLAN.
- VLANs 4, 11, 50, 88, and 99 are tagged.
- VLAN 50 is no longer configured as untagged on that trunk.

## Validation

- Sandbox VMs received valid DHCP addresses.
- Connectivity on VLAN 50 was restored.
- Existing production and other VLANs continued to operate normally.

## Prevention

When changing a switch port from access mode to trunk mode, review its complete VLAN
membership and PVID. Confirm that exactly one VLAN is untagged and that every additional
VLAN is tagged.
