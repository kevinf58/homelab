# 2.0 - Requirements Analysis

_This section entails all functional and non-functional requirements for the system_

## 2.1 - Functional Requirements

- User is able to upload and download data remotely from the home cloud via phone
- Portfolio website is hosted and can be accessed remotely via the domain purchased
- Centralized user auth through an Active Directory server
- Each core service must be hosted in a VM, and the VMs must be able to start, stop, and be restored and backed up independently
- Physical display of live hardware health and metrics with notifications for abnormal conditions or potential hardware failure.

## 2.2 - Non-functional Requirements

- 24/7 uptime with the exception of a scheduled maintenance period
- Automatic restart and restoration of core system functionality for all services in the event of an unexpected reboot
- All communications between the phone and the cloud must be encrypted
- Reliable power (in the form of a UPS) and automated notifications in the event of power failure
- Continuous operation of cloud storage in the event of a drive failure
- Scalability in the context of being able to expand cloud storage by adding more drives to my DAS and easy deployment of new VMs
- Easy maintainability of VMs and services through a central interface
- Monitoring hardware health and metrics with notifications of abnormal conditions or potential hardware failure
