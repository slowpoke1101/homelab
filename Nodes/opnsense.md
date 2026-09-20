# Node Profile — Mimi

**Hardware:** Intel NUC7i5BNK  
**Upgrades:** 16 GB RAM, 256 GB NVMe, Realtek Ugreen 2.5GbE USB NIC for WAN  
**Role:** Gateway and firewall  
**Version:** OPNsense  
**Network:** Native LAN on VLAN 1 with VLANs 4, 11, 50, 88, and 99 tagged

---

## Services
- Unbound DNS
- DNSMasq DHCP
- Suricata IDS
- Firewalling and routing
- VLAN-aware access control between trusted, production, management, and isolated networks

---

## Key configurations
- LAN is on the native NIC and the switch carries the tagged VLANs for production, management, sandbox, roommate’s LAN, and IoT.
- The WAN side is the Realtek Ugreen USB NIC so the firewall can sit cleanly on the building-managed upstream connection.
- The firewall uses default-deny style thinking for the network segments and only allows the traffic the nodes actually need.
- DNS overrides and lightweight ad-blocking are enabled in Unbound for internal services.

---

## Purpose
This node is the router and first security boundary for the homelab. It does the heavy lifting for routing, DNS, IDS, and network segmentation.

---

## Future Improvements
- More advanced firewall tuning and review of allowed services
- Better documentation of the exact rulesets and traffic flows
- Additional hardening and rule review as the environment grows

![OPNsense Console](images/opnsense_console.png)
