# 05 - Preparing for the Future: Notes, Updates, Backup Data, Backup Power, and Upgrades

Computers aren't a *set it and forget it* endeavor. They require regular maintenance and protection against future elements. You have just spent some number of hours learning about servers and linux and code, but if you don't keep it up everyday eventually you're going to 

## Notes
You will thank your future self if you take detailed notes while you setup your server. Try and be as thorough as possible, even if you think you won't forget it (you probably will). 

Consider making a record of the following:

- Any deviations made from this guide
- Draw a "map" of your folders and files.
- Draw a "map" of your physical devices (like my drawing).
- Your router settings and IP addresses.
- Organize your passwords (I use 1Password).

## Updates
[Forthcoming]

## Backup Data
SBCs like the RockPro64 are awesome little devices that are great for learning and experimenting on the cheap, but they're also more likely to break on you than a $4,000 MacBook Pro. Be prepared for hardware and user error and have a backup plan.

[More on this coming...]

## Backup Power
I highly recommend getting a **Universal Power Supply** (UPS). SBCs are particularly sensitive to inconsistent electricity. A UPS will help ensure that your server is receiving clean, reliable power at all times.

Additionally, sudden loss of power can result in data loss or program malfunctions. A UPS also protects against those events.

The size of the UPS is up to you. I have two: one for my modem, router, and a couple of other SBCs; and another for the server itself.

Be sure and get one with **pure sine wave output** as this is best for sensitive electronics.

My RockPro64 Server only pulls about 5 to 8 Watts and the UPS has a 600W battery. In the event of a power outage, the UPS can keep the server up for 600W / 8W = 75 hours! Plenty of time to get home and fix the problem or safely shut the computer down.

## Upgrades
Some upgrades to look into:
1. Docker Swarm
2. Additional Security
3. More Applications and Use Cases
