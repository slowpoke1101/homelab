# Node Profile — Flint2 wireless access point

**Hardware:** GL.iNet Flint 2 (GL-MT6000), two 2.5GbE and four 1GbE Ethernet ports  
**Role:** Wireless access point for the homelab and household networks  
**OS:** OpenWrt, configured in access point mode  
**Network:** Switch port 8; VLAN 1 untagged for management and trusted LAN, VLANs 88 and 99 tagged

---

## Services
- Wireless access for the trusted/LAN, roommate/guest, and IoT networks
- Layer 2 bridging of wireless clients onto their assigned VLANs
- LuCI interface for configuring VLAN-aware networking and wireless interfaces

---

## Key configurations
- The second 2.5GbE port connects to switch port 8.
- The uplink carries VLAN 1 untagged and VLANs 88 and 99 tagged. The Flint2 bridge maps these VLANs to their wireless networks:

  | Wireless network | VLAN | Uplink |
  | --- | --- | --- |
  | Trusted / LAN | 1 | Untagged |
  | Roommate / guest | 88 | Tagged |
  | IoT | 99 | Tagged |

- The Flint2 is in access point mode. OPNsense and the managed switch provide routing, DHCP, firewall policy, and VLAN transport.
- The management interface is on VLAN 1 and receives its address from OPNsense. The current address is not recorded here.
- OpenWrt's LuCI interface is used to configure unmanaged VLAN interfaces, bridge VLAN filtering, and SSID-to-network assignments.
- Client isolation between VLANs is enforced by OPNsense firewall rules, not by the access point.

---

## Purpose
The Flint2 provides wireless connectivity while keeping the homelab's existing VLAN segmentation. It lets trusted, roommate/guest, and IoT devices use separate networks without making the access point responsible for routing or firewalling.

---

## Future Improvements
- Record the current DHCP reservation for the management address.
- Document the active wireless security settings and radio/SSID details.
- Revalidate client addressing, VLAN isolation, and management access after network changes.

---

## Related documentation
- [Network topology and VLANs](../Architecture%20Overview/network-topology.md)
- [Flint2 access point deployment runbook](../Runbooks%20%26%20Projects/DEPLOYMENT-wirelessaccesspoint/Deployment.md)
- [Managed switch profile](switch.md)
- [OPNsense profile](opnsense.md)
