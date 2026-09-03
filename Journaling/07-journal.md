# Updates
It's been almost 2 weeks since my last journal. I've been a bit busy lately, so I haven't had a lot of time to write or work on my home lab.  I have however been reading up a lot on a lot of articles and I've gotten multiple steps closer to completing a rough draft of everything. Once that is done, I'll be able to continue the design phase of my SDLC before finally moving onto implementation!

To start off, I've decided to refund my switch. The primary function for this device is to separate my other machines into different VLANs so that with the proper security config, if one machine is compromised, the others stay unaffected. I only have one machine. I can always reconsider the switch purchase in the future when I scale this project into multiple machines.

Avoid installing Tailscale directly on the Proxmox host and install it on the VMs and LXCs that need to connect to my Tailnet instead. A lot of my services are going to rely on Tailscale for Auth, so if my Tailet gets compromised, associated LXCs and containers are in danger as well. With the right firewall and access control setup, I'm confident I'll be able to minimize the impact of an intruder gaining access to my Tailnet. Honestly, I don't know what I was thinking when I said that I'd install it directly on the host in the last journal.

## Portfolio Website
The site is static, so its setup is pretty minimal. What I'll need first is a place to hold and serve the files that make up my website. I'm thinking of 2 options here, but leaning towards one over the other: Nginx and Caddy - both of which also have reverse proxy capabilities. At the start of this project, I thought RAM was a massive constraint that I had to keep tabs on, but I no longer think it's as big of a constraint as I thought it to be. Caddy runs in GO, so there would be more RAM overhead as opposed to if I were to run Nginx in C and it's also the more modern option.

Next, I need something to make my hosted website accessible to the public. I see 2 primary options: port-forwarding using my home router and a Cloudflare tunnel.
1. Port-forwarding using my own router opens 2 ports and exposes my home network to the public, so I'm not going to go with this option. It's a popular solution, but there are more secure modern alternatives.
2. Cloudflare tunnels are free for my use case, hides my IP, and handles my HTTPS encryption and renewals. This is the better option of the two.

Currently, I plan on using Caddy and a Cloudflare tunnel to host my website. I'll also have a private subdomain dashboard.fengkevin.com accessible only to devices connected to my Tailnet. 

## Dashboard

As I just mentioned, this will be accessible via a private subdomain. To display my dashboard using a physical monitor at home, I'll set up a browser tab (likely Chromium) running in kiosk mode. As for the tools to use for developing the dashboard itself:
1. Grafana + Prometheus + Loki: seems to be the gold standard for enterprise dashboards. I've used Grafana in the past, which is also a bonus.
2. Homepage + Netdata + Uptime Kuma: The more homelab friendly alternative that is less resource hungry. 
