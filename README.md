# RockPro64 Personal Server
Stop giving Google your information.
Stop giving Dropbox your money.
Sovereign up and run your own damn server.

## Project Background
This project began after I temporarily lost access to some bitcoin on a lightning wallet I had set up on a Raspberry Pi 4. At the time, I was interested enough to go through the process: acquiring the precisely-specced hardware, learning how to flash software, how to setup remote access, how to troubleshoot if the internet or power went out, etc. I had even purchased a UPS (Uninterruptible Power Supply) and had remote access over Tor. It had been running smoothly and without issue for months when, suddenly, it died three days after I left for a two-week vacation.
After getting back home, spending time on forums, and digging into every possible reason for its failure, I ultimately figured it was a hardware issue. I had an external hard drive plugged into the USB3 port on the Raspberry Pi and either the drive or the ports went caput. 

While "plug-and-play" devices are great, so much of running your own server is about maintenance and troubleshooting. Without skills to troubleshoot, how are you supposed to rescue? It would be like putting an eight-year-old in the driver seat of a car: they might get along fine in an empty parking lot, but as soon as they encounter their first obstacle--an on-coming car or brick wall--all is likely lost.

## Who is this guide for?
This guide is intended for the non-technical-yet-curious: those of us who want more power and privacy from Big Tech but have minimal coding experience. If you are one of those people, this guide is meant for you. It will hold your hand at each step while you code up your own server with the other.

For more on the Principles behind this guide, see Appendix I.

While only minimal computer skills are required to get through this guide you should probably already know the following (or acquire the skills elsewhere):

1. How to access and adjust your home router settings.
2. What a "command line terminal" is.
3. How to SSH into a device on your home network.
4. Good password management.
5. Willingness to work with tedious details; patience to deal with errors that are frustratingly stupid in hindsight.

The Long Now Server
A Simple Server
A Server for Tomorrow
A Server for the Strayed 
Self-Hosted Server
RockSolid
KISS
The principle is best exemplified by the story of Johnson handing a team of design engineers a handful of tools, with the challenge that the jet aircraft they were designing must be repairable by an average mechanic in the field under combat conditions with only these tools. Hence, the "stupid" refers to the relationship between the way things break and the sophistication available to repair them.
Rule of Least Power (use the smallest software you need; bigger leaves room for error)
Churchill's Server
Rubber Duck Server (spoiler: the rubber duck is your future self).
Jack of All Trades
Rubber Duck Debugging
This server will need to function both now and later. Spinning it up once is half the work; the other half is getting it back up when it goes does. And if you’re like me—someone who doesn’t work with computer code everyday—there’s a good chance you’ll at least forget the tedious details if not the big ideas. By designing a server that works for the future you (i.e., the stupider you) you will be more likely in a position to keep the server up for the long term.
This does not mean you should not experiment with adding features to your server. You should. But do it always with the KISS principle and keeping future upgrades segregated from past successes. This is what Docker does.



{{TOC}}
/01-Acquire-and-Assemble-the-Hardware.md

/02 - Install Debian OS.md

/03 - Install Docker.md

/04 - Install Applications.md

/05 - Backup Data and Power.md
