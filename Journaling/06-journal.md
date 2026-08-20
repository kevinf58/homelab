# Updates

So it hasn't been too long but a lot has happened!
 - HBA card, cables, switch, PCIe riser and adapter, both 80mm and 40mm fans, diy HDD rack, second ethernet cable came, and more arrived
 - I set everything up and it all seems to be running fine. Drives show up in Proxmox and passthrough works
 - Currently in the process of running extended health checks

Once the health checks complete and I verify that none of the drives are degraded and are safe for use, I'm going to start doing more brainstorming and research on how I want my homelab to be set up and the tools that I will use. I'll also set up my ZFS pool directly on Proxmox. I could run it in a VM with TrueNAS or Unraid which would make management easier, but I'd be sacrificing RAM and it'd be a single point of failure. Running it directly on the hypervisor seems to be better practice anyways.

## Next Steps

Next, I'm going to set up scheduled monthly scrubs. For now, these scrubs won't do anything as I haven't moved any data onto my drives yet. I'll also schedule long and short SMART tests sometime later on to get more data on the health of my drives. 

I'll also set up a lot of security infrastructure and precautions.

To begin, we have VLANs. I can think of 3 VLANs to set up 
 - Management (Proxmox hypervisor)
 - DMZ (Portfolio website)
 - Trusted VLAN (Cloud storage, dashboard)

Install Tailscale on the Proxmox host. My dashboard and cloud storage VMs will also use this so I may have to install it in those VMs as well.

Setup my Proxmox host firewall so that it's only accesible via my desktop pc and Tailscale.

At the time of writing this I realized that a switch isn't really that necessary as it doesn't provide me any benefit that Tailscale and Proxmox don't already provide. I only have one machine after all. 
