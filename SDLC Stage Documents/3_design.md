# 3.0 - Design

_The purpose of this section is to satisfy and elaborate on the requirements mentioned in the requirements analysis_

## 3.1 - General Architecture

The system consists of a single-node virtualization platform (Proxmox VE) hosting a set of isolated VMs and LXC containers, each responsible for one function. In the absence of a VLAN-capable switch, network segmentation is achieved logically through the Proxmox firewall (datacenter, node, and guest levels), strict per-guest IP/port allow-listing, and a Tailscale-based zero-trust overlay network for remote access. Public-facing traffic reaches the platform exclusively through an outbound-only Cloudflare Tunnel, so no inbound ports are ever opened on the home router.

### 3.1.1 - C4 Diagram

<ins>Context diagram</ins>

![context diagram](context.png)

<ins>Container diagram</ins>

<ins>Component diagram</ins>

## 3.2 - Hardware Design

- Lenovo ThinkCentre m920q: Hypervisor host
- 512GB SSD: Proxmox installation and VM boot disks
- 32GB RAM: Memory required to host services
- ThinkCentre PCIe adapter: x16 PCIe slot addon for expansion cards
- Ethernet cable: Proxmox support and provide faster, more stable LAN throughput
- UPS: Power protection and reliability
- Display monitor: Display for metrics and analytics dashboard
- 8gb+ bootable USB: Flash Proxmox
- SAS HBA card flashed into IT mode: Storage controller expansion card for SAS drives
- x16 PCIe extension ribbon: Extend the HBA card outside of ThinkCentre enclosure. May not be needed
- SFF-8087 to 4x SFF-8482 + 4x SATA power breakout cable: Splitting connections amongst SAS HDDs, PSU, and HBA card
- 40mm fan: HBA card cooler
- 80mm fan: HDD cooler
- AC-to-DC PSU with minimum 5 SATA ports: HDD fan and HDD cooler
- PWM controller: HDD fan remote
- 4 SAS HDD's: Cloud, RAID, and backup storage

<ins>Physical Infrastructure Diagram</ins>

## 3.3 - Storage Design

Storage is split between the internal NVMe boot disk and an external JBOD of four SAS drives, attached via the HBA in IT mode (pass-through, no hardware RAID).

NVMe SSD (in the ThinkCentre)

- Proxmox
- VM disks
- ISO images
- Containers

JBOD

The four drives are pooled into a single ZFS pool named tank, configured as RAID Z2 (dual-parity), tolerating up to two simultaneous drive failures without data loss. Pool created with ashift=12, lz4 compression, atime disabled, and POSIX ACLs enabled.

ZFS ARC is capped (rather than left at the ~50% default) to avoid starving guest memory on a 32GB host. Monthly scrubs and staggered SMART short/long self-tests are scheduled via cron and an overview of results are displayed on the dashboard.

| Dataset | Purpose |
| -------- | -------- |
| tank/vm-storage | VM virtual disks |
| tank/lxc-storage | LXC mounted volumes |
| tank/nextcloud-data | Nextcloud bulk storage |
| tank/minecraft | Minecraft world and server data |
| tank/backups | Proxmox backup targets |
| tank/logs | Monitoring LXC disk (Prometheus/Loki data) |

<ins>Storage Infrastructure Diagram</ins>

![Storage Architecture Diagram](storageArchitecture.png)

## 3.4 - VM/LXC Designs

Each service runs in its own dedicated guest to keep functions isolated and independent. Static IP addressing is used throughout, incrementing by 10 per guest on the 10.0.0.0/24 LAN.

## 3.5 - Security Design

## 3.6 - Backup and Recovery Design

## 3.7 - Analytics and Monitoring Design

Hardware and service metrics are monitored continuously to catch abnormalities before they cause data loss or downtime. The stack is built on Prometheus (metrics), Loki (logs), Grafana Alloy (collection/shipping agent on every guest and the host), and Grafana (visualization/alerting).

 - CPU utilization and temperature - collected via Alloy's exporter and hwmon sensors on the host.
 - RAM utilization - collected per guest via Alloy's exporter.
 - Readable SMART data - collected via smartctl_exporter on the host; raw attributes are abstracted into simplified pass/fail and last-self-test panels rather than shown as raw output on the main screen.
 - Network utilization - throughput, errors, and drops via Alloy's exporter.
 - HDD and VM/LXC state - ZFS pool health via zfs_exporter; guest up/down and uptime via pve_exporter against the Proxmox API.
 - Storage capacities and temperatures (ThinkCentre and DAS) - ZFS pool capacity via zfs_exporter, NVMe/CPU temps via hwmon, and SAS drive temps via SMART, since spinning SAS drives are not exposed through hwmon.
 - UPS battery state - to be integrated via NUT (Network UPS Tools) exporter.

## 3.8 - Scalability

This system is designed to be scalable if more compute resources or functionalities are needed

- HDDs can be upgraded for more cloud storage
- VMs abstract each function, allowing for the addition of more VMs for future functionalities
- RAM and can be upgraded to higher capacities
- VM boot disk storage can be upgraded via higher capacity NVMe SSDs
- Additional ThinkCentres can be purchased to support the workload
