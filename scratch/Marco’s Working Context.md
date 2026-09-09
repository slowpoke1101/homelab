Marco’s Working Context

This project is maintained by Marco, an IT professional actively building skills toward CompTIA Security+, homelab architecture, virtualization, networking, and automation.
Marco prefers clear, technical, step-by-step explanations, especially when dealing with ambiguous or stressful topics.
He values accuracy, verification, and evidence-based reasoning.

Marco’s tone is casual, expressive, and direct.
He appreciates when the assistant:

    explains architecture cleanly

    avoids drifting off-topic

    validates when something is correct

    challenges gently when something is wrong

    provides implementation-ready guidance

    uses his naming conventions for hardware and nodes

Marco’s Homelab Naming Scheme

Use these names consistently when referring to machines, configs, or architecture:
    
    Flint2 - wireless access point openwrt (gli.net flint mt6000)
    used to use flint1 as wap also but i finally figured out how to propagate the vlans/network interfaces through openwrt while the router is in ap mode :D

    Mimi — OPNsense router (NUC7i5BNK) 
    256gb nvme 16 gb ram
    ids with basic rulesets enabled, minimal adblocklists enabled on unbound dns as well as dns overrides to match my ngnx proxy entries.
    all networks are default deny firewall rules and allow all but rfc1918 and allow 53(dns) to the firewall. management and production have
    general rules allow the nodes that need to speak to each other to do so.
 
    Sora — Proxmox node v8(M75q Gen2)
    500gb nvme
    48gb ram

    Joe — TrueNAS scale (M715q)
    256gb nvme(OS)
    1tb sata ssd(zfs pool)
    16gb ram

    TK — Raspberry Pi 4 backup node rpi 0s gui
    4gb ram old laptop 1tb hdd boot drive connected with usb to sata

    observer - raspberry pi 4 metrics and log collector rpi OS gui
     8gb ram samsuung evo 500gb sata ssd as boot drive connected with usb to sata

    netopia - main Docker VM ubuntu server

    Tai — Netgear GS308Ep switch (powers both rpi4's with poe+ hats)

    LONS — iPhone 16

    Izzy — Lenovo IdeaPad 3 laptop

    various VMS that i will organize eventually

When generating examples, configs, or diagrams, use these names.
Network Topology Overview

switch ports
1 - pvid 11 joe plugged in
2 - pvid 11 -  1, 4, 50, 88, 99 tagged sora plugged in
3 - pvid 11 tk plugged in
4 - pvid 50 unused
5 - pvid 50 unused
6 - pvid 11 observer plugged in
7 - pvid 1 - 4, 11, 50, 88, 99 tagged mimi LAN plugged in
8 - pvid 1 - 88, 99 tagged flint2 plugged in and broadcasting my lan, wifes lan, and iot/guest

Marco’s network uses multiple VLANs across Tai:

    LAN (VLAN 1) — 192.168.33.0/25

    V88 (VLAN 88) — 10.1.88.0/25 (wife’s devices sensitive to jitter)

    Production (VLAN 4) — 10.4.4.0/24

    Management (VLAN 11) — 10.1.11.0/25

    Sandbox (VLAN 50) — DHCP

    IoT (VLAN 99) — 10.1.99.0/25

Mimi’s LAN is on the native NIC; WAN is on a Realtek Ugreen 2.5GbE USB NIC.
OLD topology was WAN > Flint2(wifes lan) > mimi > my network. but ive since then changed it to WAN > Mimi > everything else
i mention this because it is easy for me to switch back and forth and change configs accordingly, and this is a possibility if my wife's jitter  
issues continue.

When giving networking advice, respect this topology.
Marco’s Homelab Preferences

    Prefers affordable, practical hardware

    Prefers Intel NICs for ThinkCentre Tiny systems

    Prefers clean cabling using patch panels

    Sensitive to airflow issues inside enclosed furniture

    Budget-conscious

When suggesting hardware or architecture, follow these constraints.
Current Homelab Stack

Marco’s Docker VM runs:

    AdGuard (host-only)

    Gitea


    Immich

    LibreSpeed

    iPerf3

    Memos

    Nginx Proxy Manager( i have a wildcard cert for all my web apps and stuff for *.hub)

    StirlingPDF

    Uptime Kuma

    Vaultwarden

    Grafana

    cAdvisor
    
    node exporter (nodeexporter is also running on mimi joe and sora)

observer runs victoriametrics and loki + promtail in docker
tk runs rsync backups of truenas shares twice a day

Node Exporter runs on Sora and the Docker VM.
Joe provides SMB + NFS.
the path is /mnt/TANK/NASgul for smb and mnt/TANK/nfs for nfs. all nodes have entries in /etc/fstab for smb mount and netopia has that plus an nfs mount. nasgul is for my general file storage and nfs is where netopia backs up it's containers to.
TK performs rsync backups of Joe.

When giving advice, assume this stack exists.
Marco’s Learning Goals

    Improve networking fundamentals

    Improve homelab reliability

    Improve documentation practices

    Prepare for Security+

    Build reproducible infrastructure

    Reduce configuration drift

When answering questions, align with these goals.
Assistant Behavior Guidelines

When responding to Marco:

    Be technical, structured, and clear

    Provide step-by-step guidance when relevant

    Avoid vague or generic explanations

    Reference his existing homelab architecture

    Use his naming scheme

    Avoid assumptions that contradict his setup

    Offer diagrams or structured breakdowns when helpful

    Validate correctness before suggesting changes

    Avoid overcomplicating solutions

    Keep tone friendly, direct, and confident

Example Prompting Style

Marco may ask questions casually, express frustration, or be unsure.
Respond with calm, precise explanations and avoid drifting.
current goal is to fully document homelab on github, in a way taht is genuine to my skill level but professional enough that i can
put it on my linkedin in the hopes that a hiring manager might see it and consider me for entry level role foot in the door position

Purpose of This File

This file exists so AI tools inside VS Code understand:

    Marco’s environment

    Marco’s preferences

    Marco’s homelab

    Marco’s naming conventions

    Marco’s goals

    Marco’s style

Use this context when answering any question in this workspace.

this is confirmed the state of the homelab and will be the main source of truth to be updated untill we get changelog and stuff in place