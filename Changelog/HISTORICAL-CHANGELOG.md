# Homelab Changelog

## Historical Milestones  

## Initial Environment - Raspberry Pi 4 and Laptop

### Added

- Raspberry Pi 4 serving as primary homelab server.
- WireGuard VPN.
- Pi-hole DNS filtering.

### Changed

- Replaced native Pi-hole deployment with Dockerized Pi-hole and Unbound stack.
- Developed changes in a VirtualBox staging environment before production deployment.

### Significance

- Introduced change management practices through staging and testing.
- First separation between development and production environments.  

## Enterprise Simulation Lab Expansion

### Added

- VMware Workstation based lab environment.
- FreeIPA domain services.
- Ubuntu-based web servers.
- Internal DNS infrastructure.
- Domain-joined Linux workstations.
- DVWA security training environment.

### Changed

- Added dedicated management, server, and user networks.

### Significance

- First exposure to identity management, DNS administration, web hosting, and network segmentation.  

## VLAN Introduction and Dedicated Router

### Added

- GL.iNet Flint router running OpenWRT.
- VLAN segmentation.

### Changed

- Moved WireGuard endpoint from Raspberry Pi to router.
- Introduced trusted, untrusted, and production networks.

### Significance

- First implementation of true network segmentation and router-based VPN services.  

## Infrastructure Modernization

### Added

- OPNsense firewall.
- Managed switch.
- Proxmox virtualization host.
- TrueNAS storage server.

### Changed

- Migrated Docker workloads from Raspberry Pi to virtual machines.
- Repurposed Raspberry Pi as backup infrastructure.
- Migrated SMB storage from Raspberry Pi to TrueNAS.
- Expanded VLAN architecture.

### Significance

- Transitioned from hobbyist self-hosting to a small enterprise-style infrastructure.
- Established production workloads, backup services, and virtualization platform.  

## Infrastructure Expansion and Operations Maturity

### Added

- Secondary Raspberry Pi 4.
- PoE+ infrastructure.
- Dedicated monitoring node.
- Active Directory lab.
- Documentation workstation.
- Ollama and Open WebUI services.
- Penetration testing lab.
- Dedicated staging environment.

### Changed

- Migrated monitoring stack to observer node.
- Migrated TrueNAS from bare metal to a virtual machine.
- Converted second Proxmox host into dedicated testing and learning platform.

### Significance

- Separate production, monitoring, backup, testing, and learning functions.
- Improved observability, resiliency, and operational practices.
- Established a realistic SMB-sized infrastructure for hands-on learning.
