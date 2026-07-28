Today's Monday and I've now set my ThinkCentre in my room and routed the ethernet cable to the router on the first floor. I've done some more thinking on how I want this system designed and I wanted to mention a few things just to justify to myself why I made some decisions.

### Why I chose a DAS over a NAS

NAS's are recommended for RAID configs and remote file access, which are 2 things that I am looking to implement in my system. I also wouldn't have a single point of failure for my homelab as a NAS runs independently. Normally, it would be a given to choose a NAS, but I chose to go with a DAS mainly because of budget constraints. Proxmox prefers ZFS software raid anyways so I'll use RAID-Z1. As for remote file access, I'll have to do a few extra steps if I want this functionality in my DAS like accessing my home network using a VPN first, and then accessing NextCloud.

<ins>Key problem with this setup</ins>

No hardware RAID will exist on my DAS if I am looking to use Raid-Z1 software RAID. Using the JBOD config in my DAS means that I would be bridging the connection between my 4 drives and ThinkCentre using one USB-C cable. That cable dropping for even a second could result in corruption or loss of data. The best solution for this would be to get a JBOD enclosure that offers one eSATA connection per SATA HDD so that I could attach them all to a SATA HBA PCIe card attached to my ThinkCentre. This would be a pain in the ass and my HBA card will most likely be living out of the ThinkCentre enclosure.

<ins>What to look for in my NAS</ins>

- 3.5" SATA HDD compabibility
- 4-bay at least
- JBOD config as ZFS requires direct access to the disks
- UASP support to for higher data transfer speeds
- 10 Gbps support
- Controller that doesn't see all drives as a single unit so that pulling out a live drive or a single drive failure doesn't crash the whole system

A DAS enclosure that I've been eyeing is TERRAMASTER's D4-320 as it offers everything mentioned above and is currently on sale.

### SAS drives over SATA drives?

SAS drives are heavily discounted compared to their consumer counterparts because companies offload their drives by bulk and alot of them end up on the market for a massive discount and the standard consumer doesn't have the hardware to support a SAS drive. The SAS connection is also more reliable than a standard USB connection and it'd more closely resemble an enterprise system.

However, with these pros come alot of cons. SAS drives are less supported in normal consumer-grade systems so I do have to compensate for them. I'd have to use a HBA card on my singular PCIe slot instead of a NIC and I don't even think it'd fit the enclosure of the ThinkCentre so I'd either have to make a custom chassis for the ThinkCentre or not have a chassis at all. That singular issue is enough to drive me away from SAS drives. On top of that, I don't want a massive enclosure for my drives and I'm limited to a few small enclosures that I can choose from, and power consumption would be higher along with the heat and noise.

SAS would be a massive overkill for my current planned worloads. It may be something that I'd consider in the future (most likely not), but for now, I'm going to stick with SATA.
