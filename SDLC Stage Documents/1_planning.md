# 1.0 - Planning

_Initial objectives, scope, resources, and timeline will be identified and documented here_

## 1.1 - Goals

Goals for this project are to set up and host a home cloud storage, personal website, hardware metrics dashboard, and AD server on a Lenovo ThinkCentre m920q. Some other things to be to this project but don't come as separate functionalities is a Firewall, and a backups/logs server.

The ultimate goal is build a homelab that resembles that of an enterprise server as much as realistically possible while remaining functionally tailored to the users' needs and without too many unnecessary features.

## 1.2 - Project Scope

### In Scope

- Virtualization using Proxmox
- Hosting a personal cloud storage
- Hosting a personal portfolio website
- Monitoring hardware and infrastructure metrics
- Implementing an Active Directory environment
- Setting up a backup and logging system
- Implementing network security and firewall functionality

### Out of Scope

This project will not attempt to reproduce enterprise functionality at a large commercial scale.

- Geographically distributed data centers
- Large-scale high-availability infrastructure
- Enterprise-level redundancy beyond what is reasonable and practical for a homelab
- Infrastructure whose cost significantly exceeds the practical benefit

## 1.3 - Timeline

The project is not constrained by deadlines because it is intended to be developed incrementally as a long-term homelab project.

The initial priority is to establish the infrastructure required for the personal website and cloud storage functionality within the next 2 months.

## 1.4 - Resources

### Hardware Resources

- Lenovo ThinkCentre m920q
- External DAS (prefereably 4-bay+)
- SATA HDDs (preferably enterprise grade)
- ThinkCentre PCIe adapter
- 2.5 GbE+ NIC
- Ethernet cable
- UPS
- Mini display monitor
- 8gb+ bootable USB (to install Proxmox)

> [!IMPORTANT]
>
> ### An update to Hardware Resources
>
> My original plan identified an external DAS and SATA HDD's as the storage solution. During the design phase, I identified a single point of failure in my design and chose an alternative solution to go with.
> This alternative solution would require the following updated list of hardware:
>
> - Lenovo ThinkCentre m920q
> - ThinkCentre PCIe adapter
> - Ethernet cable
> - UPS
> - Mini display monitor
> - 8gb+ bootable USB (to install Proxmox)
> - SAS HBA card flashed into IT mode
> - x16 PCIe extension ribbon
> - SFF-8087 to 4x SFF-8482 + 4x SATA power breakout cable
> - 40mm fan
> - 80mm fan
> - AC-to-DC PSU with minimum 5 SATA ports
> - PWM controller
> - 4 SAS HDD's
>
> Despite this change in resources, the initial objectives for this project remain unchanged.

### Financial Resources

The initial budget for the homelab sits at around $2,000 but it will most likely increase as it scales in the future.

### Software Resources

Choices in software will be determined during the requirements analysis and design phases based on the functional and technical requirements.

## 1.5 - Constraints

### Financial Constraints

All the funds being invested into this come from the developer's wallet, so money will always be a constraint.

### Network Constraints

The capabilities and bandwidth of the developer's home network may limit hosting performance and other network-dependent functionality.

### Hardware Constraints

The ThinkCentre m920q and other available hardware impose limitations on CPU resources, RAM, storage, networking, and power consumption.

### Operational Constraints

Because the infrastructure is hosted in a residental environment, the service availability of the system may be affected by power outages, internet outages, and other environmental factors.

## 1.6 - Risks

| Risk                           | Potential impacts                                            | Mitigation                                                 |
| ------------------------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| Hardware failure               | Loss of services or data                                     | Backups and monitoring                                     |
| Power outage                   | Service interruption or hardware issues                      | UPS                                                        |
| Internet outage                | Loss of remote access                                        | N/A                                                        |
| Insufficient storage           | Inability to make writes to cloud                            | Expand storage when required                               |
| Insufficient compute resources | Reduced performance or inability to host additional services | Monitor resource usage and pgrade hardware whjen justified |
| Security vulnerabilities       | Unautorized access or data compromise                        | Firewall, updates, access controls, monitoring, SSL        |

## 1.7 - Feasibility

This project is technically feasible because all services proposed can be hosted using a virtualization platform on the available hardware, with additional storage hardware added as necessary.

As for financial feasibility, the budget for this project starts at $2,000 with future purchases being made only when their expected benefits justify their costs.

Some enterprise-level features are considered unreasonable for the project's current scale. For example, maintaining multiple geographically distributed data centers for disaster recovery would provide additional resilience but would not be proportionate to the budget or scale of this homelab.

This project will therefore focus on a balance between enterprise-like architecture and features and practical homelab requirements.

## 1.8 - Success Criteria

This project will be considered successful when the following holds:

- The planned core services are fully functional and accessible to their intended users
- Services can be deployed and managed in a structured manner
- Infrastructure performance and resource utilization can be monitored
- Data can be backed up and recovered
- The infrastructure is sufficiently documented to allow future maintenance and expansion
- New functionality can be added without requiring unnecessary redesign of the entire environment
- The overall infrastructure provides meaningful value relative to its cost and complexity
