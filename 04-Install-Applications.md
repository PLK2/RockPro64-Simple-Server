# 04 - Install Applications

This guide draws heavily from another: [https://github.com/BaptisteBdn/docker-selfhosted-apps](https://github.com/BaptisteBdn/docker-selfhosted-apps). You should read through that now to familiarize yourself with the applications, their uses, and general setup. 

Go ahead and copy that entire GitHub repository over to your server so you have all the base files and folders ready to go. 

SSH into your server as "bob" then navigate to */raid*:
`cd /raid`

While in that directory, clone the GitHub repository. We want to avoid saving anything else on the microSD (that should only have the Debian OS and Docker installed on it). Our RAID should have  all our application files and data, so get used to saving things in the */raid* directory:
`git clone https://github.com/BaptisteBdn/docker-selfhosted-apps.git`

> Note: A number of programs referenced in the BaptisteBdn guide do not work on a RockPro64. I had to find workarounds or alternative programs, which follow below. You are certainly welcome to explore more within that guide.

After you've copied everything over, what I'll add here is the exact *.yml* and *.env* files that work for my RockPro64 server, so you can simply copy/paste, modify some of the variables for your specific circumstance, and you'll be on your way.

## But first, buy a domain name

Your server needs a domain name; you can procure one from a registrar. I'm using Namecheap.com, but you can use whomever you'd like; just know that a different registrar will handle things slightly differently so be prepared to translate as needed.

This guide will make reference to "example.com". Whenever you see that, just replace it with your own domain. To be clear, you can use any domain you'd like: *2h9d234rg87.com*, *neontattoo.net*, *purplerain.paris*, whatever. Just know that we're going to be assigning *subdomains* to each of our apps (e.g., for the *ArchiveBox* application, we might create an *archive* subdomain, which would look like *archive.purplerain.paris*).

Once registered, it takes about a day for your domain to propagate around the world.

For Traefik (one of the apps you'll soon be installing), we're going to need an *API key* from Namecheap (or your specific registrar). Log into Namecheap account, go to *Profile*, select *Tools*, then under the *Business & Dev Tools* section click the *Manage* button. Turn on API access and the API key will appear (a long string of numbers and letters). 

Next, add your *public IP address* to Namecheap's *Whitelisted IPs* below the API key. You can do that by clicking the *Edit* button.

Now go to your *Domain List* and click *Manage* next to your domain. Select *Advanced DNS*. Under the *Host Records* section, this is where you'll be adding your subdomains. To do that, click *Add New Record*, select *A Record* from the dropdown menu, replace *Host* with your subdomain, then replace *IP address* with your public IP address. And that's it.


## Required Apps
These applications will allow your server to run smoothly and securely. I recommend you start with these.

For *Traefik*, *fail2ban*, *Gotify*, and *Watchtower*, read through the *BaptisteBdn guide* referenced above to learn about those programs, then click on the links below to access my `.yml` files.

*Duplicati* is a program I found as a replacement for BaptisteBdn's *Borg*. Borg won't run on a RockPro64 as I couldn't find an arm64 version. When you click on that link below, you'll see a bit more information on it.

* [Traefik](traefik/)
* [fail2ban](fail2ban/)
* [Gotify](gotify/)
* [Watchtower](watchtower/)
* [Duplicati](duplicati/)

## Recommended Apps

### File Browser
### Wordpress
### Flame
### ArchiveBox
### Syncthing
### Wiki.js
### Pi-Hole


