# Updates

MY ATX PSU CAME 🙀

In my last journal I think I mentioned that I'd get a physical firewall to sit between the router and managed switch but wow are they expensive. I'm most likely to just go with a free software firewall because I'm poor. I'll still get the managed switch as I need to set up a DMZ for portfolio website viewers.

Also, I've also done alot more thinking in regards to where my HBA card will sit and how my storage situation will work.

## HBA Card positioning

So I think I mentioned somewhere that my LSI 9211 8i HBA card would run too hot even with the heatsink that it comes with. A few solutions come to mind:

1. Get a small blower with its exhaust pointed at the heat sink to move the hot air out of the enclosure.
2. Cut a hole into the top of my m920q enclosure and use a standard 40mm Noctua fan to blow air directly on top of the heat sink.
3. Route my HBA card outside of my enclosure using an HBA card and possibly 3d-print a enclosure with good airflow for my HDDs, PSU, and ThinkCentre to live in.

Solution 2 seems the least appealing of the 3 for me.

## More Software Talk

Last time I wrote about how I wanted my overall system architecture to be designed. After alot of reading, researching, and learning later, I think I have enough of an understanding on how I want my storage architecture to look.

As I may have mentioned in a previous journal, I've chosen to go with ZFS and purchased my HBA card flashed into IT mode for direct pass through of the disks connected. I made this decision during the early planning stages of my homelab, and after alot more reading, I'm going to stick by this decision as I want to maximize data integrity. However, my decision to go with this option comes with downsides as well. I'm going to briefly discuss the choices I had and then talk a bit more about ZFS.

### Hardware RAID

Uses a dedicated controller card to manage disk arrays independent from the host machine. Offers independent booting and saves on compute resources of the host machine, but falls short with vendor lock-in and single point of failure, and no filesystem-level integrity.

### btrfs

Uses copy-on-write, which involves writing changes of data to a new location on the disk instead of overwriting data in place. Pros include checksumming on every block, instant snapshots, etc, with the cons being unstable RAID 5/6 and file fragmentation on heavy random-write workloads.

### mdadm

The Linux kernel's built-in RAID implementation. Preinstalled everywhere, low system overhead, flexible management, but no bit-rot reduction, no checksumming, and slow rebuild of arrays.

### ZFS

A vombined file system and volume manager. Scalable, checksums, instant snapshots, built-in volumene management with RAIDZ, self-healing pools, and overall maximum data integrity. However, much higher system overhead, ECC RAM strongly recommended, inflexible pool expansion (shrinking pools or removing drives once build is hard), and performance degradation nearing max capacity. Often regarded as the gold standard.

I also recently read an article on ZFS (https://jrs-s.net/2015/02/03/will-zfs-and-non-ecc-ram-kill-your-data/) which best explains the "Scrub of Death" idea and why ZFS doesn't become uniqely dangerous if I pair it with non-ECC RAM. As for the RAM constraint, the minimum recommended RAM is 1GB per 1TB of storage for ZFS's memory cache ARC and a feature called deduplication. Deduplication saves storage space by looking for identical data blocks and deleting them. As for ARC, I'll explicitly set a cap for the amount of RAM that it can consume, which would lean a read performance hit rate drop, but it's a metric that I'm willing to tweak if RAM becomes a real constraint.

## Redundancy Options

Because I chose ZFS, the options I have for redundancy are Mirroring, and RAID Z1/Z2/Z3.

Mirroring matters only when IOPS speed matter over capacity and RAIDZ3 would require 3 of 5 drives I would be using for parity storage. Both of these options would not be reasonable for my use case, which leaves me with RAID Z1/Z2. Storing critical data alone should convince me to go with RAIDZ2, but I'm also unwilling to sacrifice 50% of my drive storage for parity so I'm split between the 2 options. Let's go into more detail about my specific situation.

Currently, I have 4 4TB SAS HGST HUS724040ALS640 HDDs that I plan to use for data storage on my machine. These drives are rated for a 1 in 10<sup>15</sup> non-recoverable error rate (URE) with an annualized failure rate (AFR) of 0.44%, equating to a Mean Time Between Failures (MTBF) of 2,000,000 hours.
