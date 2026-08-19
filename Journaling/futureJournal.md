# Updates

From the last journal, I created the ZFS pool in RAID10, created the datasets with the compressions and record sizes that I set I could create since the last time this was mentioned, and added temporary automated scrubbing using cron to run a scrub at 1am on the first of every month. I also capped ZFS arc memory usage tested the rebooting of my new pool, registered the pool as proxmox storage, and simulated a disk failure while my pool was still empty.
