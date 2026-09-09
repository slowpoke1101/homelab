# Security Architecture

## Security Boundaries

OPNsense is the enforcement point between the homelab VLANs. The baseline policy is default deny between networks, with explicit rules added for required services. Most user and IoT networks can reach the internet while private RFC1918 destinations remain blocked unless a specific exception is required.

| Boundary | Control | Intended result |
| --- | --- | --- |
| WAN to homelab | OPNsense WAN policy, private-network and bogon blocking | No unsolicited inbound access from the building WAN |
| VLAN to VLAN | Default-deny firewall rules | Segmented networks cannot communicate by default |
| IoT to trusted networks | IoT VLAN 99 isolation | Smart-home and guest devices cannot reach internal services |
| Administration to infrastructure | Management VLAN 11 and restricted firewall rules | Management interfaces are available only to authorized administration |
| Remote user to homelab | Tailscale and restricted routes | Remote access does not require a public IP or port forwarding |

## Firewall Baseline

Each user VLAN starts with these rules, in order:

1. Allow DNS to OPNsense.
2. Allow outbound traffic while excluding RFC1918 private address space.
3. Deny remaining traffic.

Production and management rules include additional narrow exceptions for systems that must communicate, such as monitoring, storage, backups, and administration.

## Administrative Access

The management interfaces for OPNsense, TrueNAS, Proxmox, and the switch are restricted to the Fedora workstation on the management network. SSH is disabled on the infrastructure nodes where local console access is available. Access rules should be reviewed whenever a new administrative device or service is added.

## Detection And DNS

- Suricata IDS is enabled on OPNsense with a basic ruleset.
- Unbound DNS provides DNS resolution and domain overrides for internal service names.
- OPNsense provides DHCP through dnsmasq.
- Nginx Proxy Manager provides TLS termination and internal routing for web services using the `*.hub` wildcard certificate.
- Loki and Promtail provide centralized log collection on observer.

## Remote Access

Tailscale replaced the previous WireGuard and port-forwarding approach because the building WAN does not provide a public IP to the condo. The current tailnet includes OPNsense and iOS. iOS access is limited to TrueNAS SMB and selected Docker VM web applications, including Immich, Vaultwarden, and Memos.

## Security Assumptions And Limitations

- This is a learning environment, not an enterprise production network.
- The building-managed WAN is outside the homelab's administrative control.
- IDS alerts require review; enabling detection does not automatically prevent every threat.
- Access rules and service exposure should be revalidated after topology, VLAN, or reverse-proxy changes.
