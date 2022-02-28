# RockPro64 Personal Server

This is a plain-language, step-by-step guide for the computer novice looking to declare independence from Google and its ilk.

## Who is this guide for?
This guide is intended for the novice coder, the non-technical-yet-curious, or the Linux greenhorn; those of us looking for an escape route from Big Tech with better privacy and personal sovereignty. If you are one of those people, this guide is for you. It will hold your hand at each step while you spin up your own server with the other.

For more on the Principles behind this guide, see Appendix I.

While only minimal computer skills are required to get through this guide, it's best if you already know the following (or acquire the skills elsewhere):

1. How to access and adjust your home router settings.
2. Good password management is a critical best practice, especially when you're taking full custody of your information.
3. Willingness to work with tedious text details and patience to deal with errors that are frustratingly stupid in hindsight.

## Project Background
This project began after I temporarily lost access to some bitcoin on a lightning wallet I had set up on a Raspberry Pi 4. It was a "plug-and-play" node which meant it was easy to set up, but after I encountered the problem I quickly realized I had no idea what I was doing. I spent a lot of time on forums, DuckDuckGo, and YouTube, trying to decipher all the code-speak aand after a while I figured out it was a hardware issue (likely the USB ports on my Raspberry Pi, or perhaps the external drive itself). 

After much headache and heartache I recovered my funds. That experience set me on a path to learn more about computers, servers, and coding, to better prepare myself for the inevitable upgrades and potential failures. In that process, I realized I could apply my new skills to build my own server. This guide is the result.

I was born in the early 80s which means I remember a time before home computers, cell phones, and internet. I'm also an architect; I didn't study computers in college and I don't work with them in a professional capacity. I'm just interested in them. As long as you share that interest, you can get through this guide.



---
At the time, I was interested enough to go through the process: acquiring the precisely-specced hardware, learning how to flash software, how to setup remote access, how to troubleshoot if the internet or power went out, etc. I had even purchased a UPS (Uninterruptible Power Supply) and had remote access over Tor. It had been running smoothly and without issue for months when, suddenly, it died three days after I left for a two-week vacation.

After getting back home, spending time on forums, and digging into every possible reason for its failure, I ultimately figured it was a hardware issue. I had an external hard drive plugged into the USB3 port on the Raspberry Pi and either the drive or the ports went caput. 

While "plug-and-play" devices are great, so much of running your own server is about maintenance and troubleshooting. Without skills to troubleshoot, how are you supposed to rescue? It would be like putting an eight-year-old in the driver seat of a car: they might get along fine in an empty parking lot, but as soon as they encounter their first obstacle--an on-coming car or brick wall--all is likely lost.