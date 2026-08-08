# Updates

I've recently made several purchases for my JBOD and am in the process of writing my design doc while I wait for everything to arrive.

I didn't mention anywhere in terms of specifics of what components I ended up purchasing so here's a list of everything:

- 5x HGST HUS724040ALS640 SAS HDD's (Seller gave me an extra one so I'll use 4 of the best out of the 5!)
  ![HGST HUS724040ALS640](image-4.png)
- LSI 9211-8I SAS HBA Card: One of the most popular SAS cards for builds like mine. Can support up to 8 SAS HDDs via 2 mini-sas ports.
- SAMA GP650 650W ATX 3.1 Fully Modular Power Supply: Supports up to 6 SATA connections and is more than enough to power my 4 SAS HDD's and fan. I'll have to plug in a twist tie into pins 4 and 5 of the 24-pin motherboard connector to jumpstart this as the PSU isn't attached to a motherboard
- Other components don't really matter so I won't be mentioning them

So far in my journals, I've talked alot about the hardware side of things and now that I've purchased everything that I need for this first part of the Homelab, it's time to talk about software!

## The Software Side of Things

Theres alot to talk about and I still have lots of thinking to do, so I'll only discuss my overall system architecture for now.

### Overall Architecture

Before I start yapping, I'd like to mention why I'm going to integrate an Active Directory environment into this Homelab. I intend to benefit from this project not only through its features that it will end up providing, but also through the learning that will be done during the development. Active Directory is one of those things in IT that I'm required to learn and what better way to learn the tool than to read up on its docs and implement it myself in an environment it makes most sense existing in? For my specific situation, there may be better solutions for authentication, but in this case, I value learning the technologies necessary over looking for the best tools for every use case. If I were looking for the most optimal tool, I'd likely go with Tailscale auth.

Now to start off, I'm going to use Proxmox VE as a hypervisor to host multiple VM's where each individual service of my Homelab will be run (AD, portfolio website, cloud, etc). This approach is more scalable as I can implement future functionalities by simply hosting more VM's, failures in services and backups are isolated within their own VM's, and overall better security.

For now, I'm thinking I separate my services into the following VM's:

- Identity and access management: Using AD, accounts, groups and their permissions will be managed
- Storage: Not to be mistaken by cloud storage. This VM manages where and how data is stored. This VM would be responsible for actions like monitoring drive health, temperatures, SMART data, capacity and activity, and providing storage to my cloud storage VM, etc.
- Cloud Storage: Using NextCloud as the cloud storage host and Tailscale to securely connect to my cloud storage remotely via phone. Storage is allocated by the storage VM for this VM to use.
- Monitoring: Uses all the metrics and analytics provided by the storage VM to display on a readable dashboard. I'll likely use Grafana for the dashboard as I've used it before. This VM also handles alerts, which I'm thinking will be separated categories based on severity like "critical", "warning", "informational" etc. Theres a massive amount of information to display which is why alot of information will be abstracted. This can be done by separating dashboard sections into categories like infrastructure, storage, security, portfolio website, etc. and being able to get a more detailed overview of each category once expanded.
- Portfolio: This is an untrusted entry point and should be separated in its own separate VLAN from my other services using a firewall. Within this VM, I'll also have a reverse proxy and monitoring agent which sends the metrics to the monitoring VM.
- Logging: Not much to say here. This VM will just display any notable actions from VM's or users.
