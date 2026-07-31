# A Big Update to My Homelab Design!

Sooo I did a bit of brainstorming and found [this](http://youtube.com/watch?v=QGkqwdM0L6g) video by Hardware Haven where he documents his process on building his own JBOD (Just a bunch of disks) enclosure with SATA drives. This pretty much directly solves the issue with my 4 drives being connected via a single USB connection if I were to buy a prebuilt DAS. The only issue I had with this setup was that the SAS HBA card would most likely live outside of my ThinkCentre enclosure as there wasn't enough space.

A JBOD setup like that would be a bit more expensive than if I were to just buy a 4-bay DAS enclosure like the TERRAMASTER D4-320 I have right now, but like I mentioned before, no single point of failure with the USB connection. The simplest option to fix this would be to just cave in and buy a NAS but paying $600 minimum for a reliable one before pricing in the SATA HDD's is really unattractive.

I've done some more thinking and I think I'm going to use a similar JBOD approach the video outlined but with SAS drives! I'll return the TERRAMASTER I purchased from Amazon.

![TERRAMASTER D4-320](image-3.png)
_TERRAMASTER D4-320 (4-bay DAS)_

## A Rough Proposal

To my ThinkCentre, I attach a Lenovo PCIe riser and adapter and then SAS HBA card. If the card doesn't fit in my ThinkCentre, I can use a x16 extension ribbon and 3d print an enclosure for my card. I've been looking at the lsi SAS 9211 8i, which provides 2 mini SAS ports each of which can support 4 drives each. Currently I only want 4 drives and may expand in the future, so I only need to occupy one of these ports for now. One thing to note is that these HBA cards need to be IT flashed so that direct control and access to the drives this is connected to can be provided. I may also attach a 40mm fan to regulate temperature. Attached to one of the mini SAS port's of the HBA card will be a cable that separates the SAS connection from 4 HDD's into their separate data and power connections. The data connections all come together to form the single mini SAS port I plug into my HBA card and the power connections separate as individual SATA power connections. These SATA connections then plug into a power supply capable of providing both 5V and 12V DC power. Also connected to the power supply via SATA is a PWM controller that is attached to an 80mm fan so that I can control the speed of the fan it's connected to. That fan will be used to regulate temperature and air between the drives. Lastly, I may also need a master-slave power strip to power my ThinkCentre at the same time as my PSU. Though I may just leave my JBOD on 24/7 since the ThinkCentre will always be on as well.

## Hardware Requirements

- Lenovo PCIe riser and adapter
- SAS HBA card flashed in IT mode
- x16 extension ribbon (if HBA card doesn't fit in the ThinkCentre enclosure)
- SFF8087 (mini SAS) to 4x SFF8482 (SAS) and 4x SATA power cable breakout cable
- 40mm fan
- 80mm fan
- AC to DC PSU with at least 5 SATA ports
- PWM controller
- 4 SAS HDD's
- Master-slave power strip (optional)
- Some 3d prints (maybe)

This proposal would be the best balance between budget and reliability. Originally, I was looking at 4TB enterprise SATA HDD's for around 120 CAD per (times are rough), but with this implementation I'd be able to save around 40-50% on storage. Best case scenario is that the proposal does well in practice and the SAS HBA card also fits into my ThinkCentre!
