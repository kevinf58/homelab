# Updates
After weeks of reading up on docs and articles, I finally have a solid idea on the complete design of this homelab. This will probably be my last journal before I begin the implementation phase. I'll likely update my SDLC stage documents beforehand.

This journal will serve as a shallow explanation for the complete design of this homelab. As I've already mentioned the hardware side of things in previous journals and they won't change the services I'll be hosting on my machine, I won't elaborate much on them and will mostly be talking about the software side of things.

I'll end off this article by outlining my Proxmox firewall configuration.

## Broad-level Architecture
To start off, all my services will be hosted on a single machine running PVE (Proxmox Virtual Environment) as its OS, with all my services running in separate VMs and LXCs for isolation and scalability. Attached to this machine will be a JBOD (just a bunch of disks) with 4 drives. Both will be powered using a UPS with an automatic graceful shutdown plan configured for disaster prevention. Within Proxmox, I'll have a ZFS pool configured in RAID Z2, meaning that 2 of my 4x4TB drives are set aside for parity to prevent data loss and encourage redundancy. This will leave a bit under 8TB of usable storage space for my services.

My method of authorization will be via Tailscale, a zero-trust friendly VPN that will enable me to access my services over public internet using all my devices connected to my Tailnet (What Tailscale calls their private, encrypted networks). 

## Dashboard

The following table lists the metrics I will track, the tools I'll use to track them, and their reason I will track them for:

| Metric | Tool Used | Explanation |
| -------- | -------- | -------- |
| CPU, local NVMe, external HDD temperatures | Grafana Alloy |  |
| Last smartctl scan results | smartctl_exporter |  |
| Last zfs scan results | zfs_exporter |  |
| Real-time RAM/CPU consumption | Grafana Alloy |  |
| HDD storage consumption | Grafana Alloy |  |
| Local NVMe storage consumption | Grafana Alloy |  |
| Logs | Grafana Alloy |  |
