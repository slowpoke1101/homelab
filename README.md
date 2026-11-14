## Sanitization Notice

All IP addresses, VLAN IDs, hostnames, network ranges, and device names in this repository are fictional placeholders.
They do not represent my real home network.
The architecture and documentation are real, but sensitive values have been intentionally altered.  
True and specific documentation is mantained on self-hosted gitea server.

## Homelab Overview

This homelab is a self-hosted network, compute, and storage environment built as a practical way to learn how infrastructure fits together. It has grown from a Raspberry Pi and a consumer router into a set of systems with distinct jobs. The goal is not simply to run more services: it is to understand and document the full path from network design and access control through application deployment, monitoring, and recovery, while keeping the environment useful day to day.

### Why It Is Designed This Way

- **Separate networks make boundaries testable.** OPNsense handles routing, DNS, DHCP, firewall policy, and intrusion detection. A managed switch and wireless access point carry separate networks for management, production services, sandbox experiments, IoT, and other household devices. Default-deny rules and narrowly allowed traffic are intended to limit what a device or workload can reach if it is misconfigured or compromised. The sandbox provides room to experiment without treating the production and management networks as a playground.
- **Virtualization keeps workloads changeable.** Proxmox hosts the Docker VM that runs most applications, alongside separate virtual machines for testing. This keeps application changes and disposable experiments apart from the physical network and storage roles, and makes it easier to learn through building, changing, and rebuilding systems.
- **Services have a practical purpose.** The application layer includes personal tools for photos, notes, documents, and password management, as well as private Git hosting and network utilities. These real workloads make the infrastructure decisions meaningful: they exercise internal DNS, storage, reverse proxying, and access controls rather than existing only as lab demos.
- **Access is centralized and constrained.** Internal DNS and Nginx Proxy Manager provide consistent names and TLS for web applications. Because the building-managed internet connection does not provide a public address to the home, Tailscale is used for selected remote access instead of exposing services through inbound port forwarding. Network rules and application access lists further limit which clients can reach administrative and personal services.
- **Recovery and visibility are part of the design.** TrueNAS provides shared storage, while a separate Raspberry Pi receives scheduled backup copies. Another Pi collects infrastructure metrics and logs, keeping that visibility less dependent on the main application VM. The repository also documents encrypted cold-storage procedures; restore testing and stronger off-site protection remain areas to improve.

This is a learning environment, not an enterprise reference design. Its architecture changes as new ideas are tested and as operational gaps become clear. The documentation aims to distinguish the intended current design from deployment procedures, past incidents, and future improvements.

## Repository Guide

- [Architecture Overview](Architecture%20Overview/README.md) describes the current topology, security boundaries, and service placement.
- [Nodes](Nodes/README.md) documents the physical systems and their roles.
- [Service Architecture](Architecture%20Overview/service-architecture.md) describes application placement, storage dependencies, backups, and monitoring.
- [Runbooks & Projects](Runbooks%20%26%20Projects/README.md) contains deployment and operating procedures, along with experiments and planned improvements.
- [Troubleshooting](Troubleshooting/README.md) records investigations, causes, and fixes for problems encountered in the lab.
- [Changelog](Changelog/CHANGELOG.md) tracks changes that affect the architecture, security, availability, or recovery.  

### Logical Network Topology  
  
![network map](<Architecture Overview/images/logical-topology.png>)
