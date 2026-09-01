# Updates
It's been almost 2 weeks since my last journal. I've been a bit busy lately, so I haven't had a lot of time to write or work on my home lab.  I have however been reading up a lot on a lot of articles and I've gotten multiple steps closer to completing a rough draft of everything. Once that is done, I'll be able to continue the design phase of my SDLC before finally moving onto implementation!

To start off, I've decided to refund my switch. The primary function for this device is to separate my other machines into different VLANs so that with the proper security config, if one machine is compromised, the others stay unaffected. I only have one machine. I can always reconsider the switch purchase in the future when I scale this project into multiple machines.

Avoid installing Tailscale directly on the Proxmox host and install it on the VMs and LXCs that need to connect to my Tailnet instead. A lot of my services are going to rely on Tailscale for Auth, so if my Tailet gets compromised, associated LXCs and containers are in danger as well. With the right firewall and access control setup, I'm confident I'll be able to minimize the impact of an intruder gaining access to my Tailnet. Honestly, I don't know what I was thinking when I said that I'd install it directly on the host in the last journal.

## Dashboard

