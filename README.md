## Sanitization Notice  

All IP addresses, VLAN IDs, hostnames, network ranges, and device names in this repository are fictional placeholders.
They do not represent my real home network.
The architecture and documentation are real, but sensitive values have been intentionally altered.  
## Homelab Overview  

The purpose of this homelab is to learn, experiment, and apply new concepts, functions, and technologies in a hands‑on environment. This iteration expands what I originally built with a Raspberry Pi 4 and a GL.iNet Flint router into a set of purpose‑built network devices.

I wanted a dedicated router, switch, NAS, and compute node, so I purchased a NUC7 mini PC, a managed switch, and two ThinkCentre Tiny PCs to add to my existing Raspberry Pi 4 and Flint routers. With this hardware, I could finally configure everything I’ve been wanting to try, while deepening my interests in virtualization, networking, and Linux.

My plan was:

    run a firewall bare‑metal on the NUC

    configure access and trunk ports on the switch

    run Proxmox on one ThinkCentre

    run TrueNAS bare‑metal on the other

    use Flint2 in AP mode as the wireless access point

I wanted real VLANs and segmented networks: home/trusted, IoT, production, management, and sandbox (with a possible DMZ/honeypot in the future).

After moving to a new condo, the building manages the internet connection, so we no longer have a public IP assigned to our unit. I needed a solution for remote access to my self‑hosted apps and services without relying on WireGuard. I also wanted to implement backups, monitoring, and logging.

I was able to accomplish all of this and more. The past couple of months have been extremely fun and exciting, and there’s still so much more to do. The documentation of what I’ve done, and what I plan to do, is in this repository.

Lastly, I’m new to GitHub and Markdown, so please excuse any rough formatting.
