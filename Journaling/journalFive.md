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

Uses a dedicated controller card to manage disk arrays independent from the host machine. Offers independent booting and saves on compute resources of the host machine, but falls short with vendor lock-in and single point of failure, slow speeds, and no filesystem-level integrity. This sucks - especially if I'm on a budget and can only purchase budget RAID cards.

### btrfs

Uses copy-on-write, which involves writing changes of data to a new location on the disk instead of overwriting data in place. Pros include checksumming on every block, instant snapshots, etc, with the cons being unstable RAID 5/6 and file fragmentation on heavy random-write workloads.

### mdadm

The Linux kernel's built-in RAID implementation. Preinstalled everywhere, low system overhead, flexible management, but no bit-rot reduction, no checksumming, and slow rebuild of arrays. I want data integrity so checksumming is a requirement, making this an invalid option for me.

### ZFS

A combined file system and volume manager. Scalable, checksums, instant snapshots, built-in volume management with RAIDZ, self-healing pools, and overall maximum data integrity. However, much higher system overhead, ECC RAM strongly recommended, inflexible pool expansion (shrinking pools or removing drives once build is hard), and performance degradation nearing max capacity. Often regarded as the gold standard.

I also recently read an article on ZFS (https://jrs-s.net/2015/02/03/will-zfs-and-non-ecc-ram-kill-your-data/) which best explains the "Scrub of Death" idea and why ZFS doesn't become uniquely dangerous if I pair it with non-ECC RAM. As for the RAM constraint, the minimum recommended RAM is 1GB per 1TB of storage for ZFS's memory cache ARC and a feature called deduplication. Deduplication saves storage space by looking for identical data blocks and deleting them. As for ARC, I'll explicitly set a cap for the amount of RAM that it can consume, which would mean a read performance hit rate drop, but it's a metric that I'm willing to tweak if RAM becomes a real constraint.

## Redundancy Options

Because I chose ZFS, the options I have for redundancy are Mirroring, and RAID Z1/Z2/Z3.

RAIDZ3 would require 3 of 5 drives I would be using for parity storage which isn't an option as I'm only using 4 and RAID 1 , which leaves me with RAID Z1/Z2/10.

To understand RAID failure better, there are a few concepts to consider:

- None-Recoverable Read Error (URE): The rate at which a drive fails to read a block of data during normal operation
- Annualized Failure Rate (AFR): The estimated percentage a model of drive will fail after a year of continuous use
- Poisson Distribution: A concept used to describe how many times an independent event is likely to occur over a set period of time. Say hypothetically, if I did a large number of rebuilds, the average number of UREs would describe the total number of errors I’d expect across all those rebuilds. However, the number of UREs in any individual rebuild can vary. The Poisson distribution shows how those errors are likely to be spread across individual rebuilds, rather than just looking at the total number of errors overall.

Currently, I have 4 4TB SAS HGST HUS724040ALS640 HDDs that I plan to use for data storage on my machine. These drives are rated for a 1 in 10<sup>15</sup> URE, meaning that one read failure is expected for about every 125TB of data read. The drives are rated at an AFR of 0.44.

Now let's talk about worst case scenarios:

- RAIDZ1 with 12TB of data. A drive fails and the pool needs to be rebuilt. This scenario has about a 9.2% chance of a URE and catastrophic pool failure (12 / 125 = 9.6% apply a Poisson distribution and you get about 9.2%).

  > _Note that I were to get standard consumer drives, most of them are rated 1 in 10<sup>14</sup> URE, translating to 1 URE every 12.5TB. If my pool is full with 12TB that equates to about a 62% chance of hitting a URE after a Poisson distribution._

- RAIDZ2 with 8TB of data. As more than 2 drives failing during the rebuild process is extremely low with an AFR of 0.44, we say that worst case it takes 24 hours to fully rebuild. This number is some 1 in 3 million per year. Rebuild speeds for this process would be slow.

- RAID10 with 8TB of data. Any 1 drive can be lost, 2 drives can be lost if they are part of different mirrors. Data loss means that 2 drives from the same mirror failed. The rough chances of this happening over a 24 hour rebuild period would be about 1 in 25,800 per year. Rebuild speeds for this process would be fast.

In my eyes, the cost to risk gap is significant between the 3 options. A 9.2% chance of losing all my data after a drive failure is not a gamble that I'd be willing to take even if I'd have to set aside 4 more TB of usable storage for parity purposes, which leaves me with RAIDZ2 and RAID10. Both of the remaining options handle the worst case scenario for a worst case scenario (UREs during a rebuild process after a drive failure). The difference is that RAID10 has higher IOPS, faster rebuilds, faster read/write speeds, but less data integrity. IOPS isn't really a metric I care about for my use case, but I do care about read/write speeds and rebuild speeds (longer window for another drive to fail). I still want to maximize data integrity, but both options already have a very low rate of a catastrophic event occurring so RAID10 seems like the better option overall.

Of course, this is all in the context of multiple nested worst case scenarios, which will most likely not happen. UREs seem to be a worst-case benchmark provided by manufacturers under extreme testing, but I'd rather be safe than lose all my data.

## ZFS Datasets

Datasets in ZFS are treated as their own individual file systems. My ZFS pool will store data from various categories, so creating datasets for each is a given. Record sizes are also an optional parameter I can add when creating these datasets and can optimize throughput. As for compression methods, zstd for logs as they are small, frequent writes and default to lz4 for everything else.

| Dataset             | Record Size | Compression Method |
| ------------------- | ----------- | ------------------ |
| logs                | 16k         | zstd               |
| backups             | 128k        | lz4                |
| nextcloud           | 1M          | lz4                |
| nextcloud/media     | 1M          | lz4                |
| nextcloud/documents | 128k        | lz4                |
