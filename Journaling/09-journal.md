# Updates
After weeks of reading up on docs and articles, I finally have a solid idea on the complete design of this homelab. This will probably be my last journal before I begin the implementation phase. I'll likely update my SDLC stage documents beforehand.

This journal will serve as a shallow explanation for the complete design of this homelab. As I've already mentioned the hardware side of things in previous journals and they won't change the services I'll be hosting on my machine, I won't elaborate much on them and will mostly be talking about the software side of things.

I'll end off this article by outlining my Proxmox firewall configuration.

## Broad-level Architecture
To start off, all my services will be hosted on a single machine running PVE (Proxmox Virtual Environment) as its OS, with all my services running in separate VMs and LXCs for isolation and scalability. Attached to this machine will be a JBOD (just a bunch of disks) with 4 drives. Both will be powered using a UPS with an automatic graceful shutdown plan configured for disaster prevention. Within Proxmox, I'll have a ZFS pool configured in RAID Z2, meaning that 2 of my 4x4TB drives are set aside for parity to prevent data loss and encourage redundancy. This will leave a bit under 8TB of usable storage space for my services.

My method of authorization will be via Tailscale, a zero-trust friendly VPN that will enable me to access my services over public internet using all my devices connected to my Tailnet (What Tailscale calls their private encrypted networks). 

## Dashboard
This service will be hosted in an LXC with Tailscale running for remote access.

I'll be using Grafana Alloy for both metrics and logs collection instead of node_exporter and the deprecated Promtail.

The following table lists the metrics I will track and the tools I'll use to track them.

| Metrics |
| -------- |
| CPU, local NVMe, external HDD temperatures |
| smartctl data |
| ZFS pool stats |
| smartctl and ZFS scan/scrub history |
| Real-time RAM/CPU consumption |
| HDD storage consumption |
| Local NVMe storage consumption |
| Logs |
| HDD I/O |
| VM/LXC uptime status |
| Last backup status and date |
| UPS/power |

As for collectors, I have 
| Collectors |
| -------- |
| Grafana Alloy |
| pve-exporter |
| smartctl_exporter |
| zfs_exporter|
| Custom Prometheus textfile collectors |
| NUT-exporter |


Besides the collectors, I'll use Prometheus as a centralized metrics collection storage for the collectors, Loki as the centralized Logs collection storage, and Grafana for data querying and building the panels that will make up my dashboard. I'll then most likely route my Tailscale MagicDNS domain to Grafana's port securely so that I can view my dashboard via the domain on all machines connected to my Tailnet.

Lastly, a rough draft of the panels I'll have on my final dashboard will not be in this journal but on the design phase document of my SDLC.

## Portfolio Website
This will be run in an LXC. In short, I'll have nginx running on a local port, create a tunnel with the local port and then route the traffic to that local host. Tailscale will be run on this LXC so that I can access it remotely.

## Nextcloud
Again, not really much to say here. I'll partition a part of my pool for Nextcloud storage and run it in a Debian VM in a Docker container and run Tailscale so that I can access the VM remotely.

## Firewall Configuration
All LXC's and VM's on my Proxmox host should be able to access the Proxmox Web UI (Port 8006) and SSH (Port 22) via my Tailnet.
