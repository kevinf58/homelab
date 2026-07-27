# 3.0 - Design

_The purpose of this section is to satisfy and elaborate on the requirements mentioned in the requirements analysis_

## 3.1 - General Architecture

C4 Diagram
Context diagram
Container diagram
Component diagram
Hardware Design

- Lenovo ThinkCentre m920q: Hypervisor host
- 512GB SSD: Proxmox installation and VM boot disks
- DAS: Storage bay
- SATA HDDs: Cloud storage
- UPS: Power protection and reliability
- 2.5 GbE NIC: Faster LAN throughput
- Ethernet cable: Proxmox support and provide faster, more stable LAN throughput
- ThinkCentre PCIe adapter: x16 PCIe slot addon for NIC
- Display monitor: Display for metrics and analytics dashboard
- 8Gb+ USB: Flash Proxmox

Physical infrastructure diagram

## 3.2 - Storage Design

NVMe SSD (in the ThinkCentre)

- Proxmox
- VM disks
- ISO images
- Containers

External DAS

- Cloud storage
- RAID pool
- Backups
- Logs

Storage architecture diagram

## 3.3 - Network Design

Network topology diagram

## 3.4 - VM Designs

## 3.5 - Security Design

## 3.6 - Backup and Recovery Design

## 3.7 - Analytics and Monitoring Design

Hardware metrics should be constantly monitored for abnormalities and disaster prevention. Some of these metrics may be monitored in the form of a graph to improve readability.
Some of these metrics may include:

- CPU utilization and temperature
- RAM utilization
- Readable SMART data (likely abstracted)
- Network utilization
- HDD and VM state
- Storage capacities and temperatures (for both ThinkCentre and DAS)
- UPS battery state
  Display these metrics on the dashboard's default screen.

If any abnormalities are found, notifications will be sent through email and/or app

## 3.8 - Scalability

This system is designed to be scalable if more compute resources or functionalities are needed

- HDDs can be upgraded for more cloud storage
- VMs abstract each function, allowing for the addition of more VMs for future functionalities
- RAM and can be upgraded to higher capacities
- VM boot disk storage can be upgraded via higher capacity NVMe SSDs
- Additional ThinkCentres can be purchased to support the workload
