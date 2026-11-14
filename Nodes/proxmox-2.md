# Node Profile — Proxmox-2 staging and lab

**Hardware:** Lenovo ThinkCentre M75q Gen 2  
**Role:** Proxmox virtualization host for learning, staging, and testing  
**Network:** Switch port 1; management on VLAN 11 (PVID/untagged), with VLANs 1, 4, 50, 88, and 99 tagged

---

## Purpose

Proxmox-2 is the homelab's staging and learning environment, not the host for the
day-to-day production services. It replaced the bare-metal TrueNAS host, a Lenovo
ThinkCentre M715q. TrueNAS was moved to a VM on [Proxmox-1](proxmox-1.md).

Changes are staged here before they are considered for the main environment. The
host is also used to learn unfamiliar technologies and test infrastructure changes
without treating the production environment as a playground.

## Network placement

The host's switch connection is a trunk. The management network is VLAN 11, carried
untagged with PVID 11; the other configured VLANs (1, 4, 50, 88, and 99) are carried
tagged. Lab and staging VM network interfaces are assigned to the sandbox using
VLAN tag 50 in their Proxmox network settings.

The sandbox is `10.1.50.0/25` and is intended for isolated testing. Keep experimental
VMs on VLAN 50 unless a specific test requires a different network and its access
has been deliberately reviewed. The physical trunk also carries production and
other VLANs, so VM VLAN assignment and firewall policy remain important boundaries.

## Current lab and staging use

### Active Directory learning lab

The current small Windows domain lab includes:

- A domain controller VM providing Active Directory Domain Services and DNS.
- One domain-joined Windows 10 VM and one domain-joined Windows 11 VM.
- Basic organizational units, user accounts, and security groups.
- Starter Group Policy settings, including disabling Control Panel access and
  wallpaper changes.
- Routine administration practice such as password resets and account unlocks.

These lab VMs use the sandbox network through VLAN 50 tags on their virtual network
interfaces.

### Infrastructure staging and testing

Proxmox-2 is used to prepare and test changes before adopting them elsewhere,
including:

- Small staged versions of infrastructure nodes.
- Network and VLAN configuration changes.
- Backup and restore testing.
- New technologies and configurations that have not yet been used in the main lab.
- Work toward a planned 1:1 replica of the environment.

The full replica is a future goal; current work consists of smaller staged systems
and targeted tests. Keep these workloads in the sandbox VLAN unless a test explicitly
requires another placement.

### Local AI and documentation workstation

The Environment includes two Ubuntu VMs:

- An Ubuntu Server VM running Ollama and Open WebUI in Docker containers. Open WebUI
  connects to the Ollama instance on this VM, and is available from to my iPhone through
  the reverse proxy.
- An Ubuntu Desktop VM used as the current primary documentation workstation.

These are lab workloads on Proxmox-2; keep their network placement and any access
between the reverse proxy and the sandbox in line with the lab's VLAN and firewall
boundaries.

### Vulnerable web application practice

The security-learning environment is being rebuilt with a Kali Linux attacker VM and
an Ubuntu Server VM running DVWA. Metasploitable 2 is a planned addition after more
study. Keep this practice environment isolated on the sandbox network and use it
only for authorized learning within the lab.

The DVWA deployment notes are in the [DVWA installation runbook](../Runbooks%20%26%20Projects/DVWA-install.txt).

## Operating boundary

Proxmox-2 is for disposable or reversible lab work. Before network or infrastructure
experiments, confirm the VM's VLAN assignment and the applicable firewall boundaries;
take a snapshot or make a backup when it will help recovery. Validate backup restores
on the lab host before relying on them elsewhere. Do not assume that being a VM on
the staging host by itself isolates a workload from other networks.

## Related documentation

- [Node inventory](README.md)
- [Proxmox-1 production host](proxmox-1.md)
- [Network topology and VLANs](../Architecture%20Overview/network-topology.md)
- [Security architecture](../Architecture%20Overview/security-architecture.md)
- [Sandbox VM runbook](../Runbooks%20%26%20Projects/sandbox-vscode-vm.md)
- [TrueNAS profile](truenas.md)