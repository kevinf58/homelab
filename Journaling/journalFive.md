# Updates

MY ATX PSU CAME 🙀

In my last journal I think I mentioned that I'd get a physical firewall to sit between the router and managed switch but wow are they expensive. I'm most likely to just go with a free software firewall because I'm poor. I'll still get the managed switch as I need to set up a DMZ for portfolio website viewers.

Also, I've also done alot more thinking in regards to where my HBA card will sit and how my networking and overarching security situation will work

## HBA Card positioning

So I think I mentioned somewhere that my LSI 9211 8i HBA card would run too hot even with the heatsink that it comes with. A few solutions come to mind:

1. Get a small blower with its exhaust pointed at the heat sink to move the hot air out of the enclosure.
2. Cut a hole into the top of my m920q enclosure and use a standard 40mm Noctua fan to blow air directly on top of the heat sink.
3. Route my HBA card outside of my enclosure using an HBA card and possibly 3d-print a enclosure with good airflow for my HDDs, PSU, and ThinkCentre to live in.

Solution 3 seems the least appealing of the 3 for me.

## More Software Talk

Last time I wrote about how I wanted my overall system architecture to be designed. After alot of reading, researching, and learning later, I think I have enough of an understanding on how I want my network and VLANs set up as well as other steps I might take to secure my homelab. From this journal onwards, I also think it's better to explain briefly on what I've learned, just so I can reflect and justify my thought process.

### VLANs

Let's start off with the VLANs I will establish.

For now,
