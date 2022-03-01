# 04 - Install Applications

> Note: This RockPro64 Simple Server guide drew heavily from another guide on GitHub: [BaptisteBdn/docker-selfhosted-apps](https://github.com/BaptisteBdn/docker-selfhosted-apps). I went through BaptisteBdn's guide and modified it to make things work for the RockPro64; additionally, I opted not to install some of the apps and did not include them here. I forked some of the other app folders and copied them here to this repository. You are of course welcome to reference that guide and make your own calls.

## Procure a domain name

Your server needs a domain name; you can procure one from a registrar. I'm using **Namecheap.com**, but you can use whomever you'd like; just know that a different registrar will handle things slightly differently so be prepared to translate as needed. You can also do this for free using [Duck DNS](https://github.com/linuxserver/docker-duckdns) but I haven't gone that route.

This guide will make reference to "example.com". Whenever you see that, just replace it with your own domain. To be clear, you can use any domain you'd like: *2h9d234rg87.com*, *neontattoo.net*, *purplerain.paris*, whatever. Just know that we're going to be assigning **subdomains** to each of our apps (e.g., for the *ArchiveBox* application, we might create an *archive* subdomain, which would look like *archive.purplerain.paris*).

Once registered, it takes about a day for your domain to propagate around the world.

For Traefik (one of the apps you'll soon be installing), we're going to need an *API key* from Namecheap (or your specific registrar). 

**To get your API Key:**
1. Log into Namecheap account.
2. Go to *Profile*
3. Select *Tools*
4. Under the *Business & Dev Tools* section click the *Manage* button. 
5. Turn on API access and the API key will appear (a long string of numbers and letters). 
6. Add your *public IP address* to Namecheap's *Whitelisted IPs* below the API key. You can do that by clicking the *Edit* button.

**To create a subdomain:**
1. Log into Namecheap account.
2. Go to your *Domain List*.
3. Click *Manage* next to your domain. 
4. Select *Advanced DNS*. 
5. Under the *Host Records* section, this is where you'll be adding your subdomains. Click *Add New Record*.
6. Select *A Record* from the dropdown menu.
7. Replace *Host* with your subdomain.
8. Replace *IP address* with your public IP address.
9. Click the _checkmark_ to save.


## Clone the Repository

Start by _cloning_ (aka, copying) the **RockPro64 Simple Server** GitHub _repository_ (aka, all the files and folders) over to your server.

1. SSH into your server as "bob" then navigate to */raid*: `cd /raid`
2. While in that directory, clone the  GitHub repository. We want to avoid saving anything else on the microSD (that should only have Debian OS and Docker installed on it). Our RAID should have all our application files and data, so get used to saving things in the */raid* directory: `git clone https://github.com/PLK2/RockPro64-Simple-Server.git`
3. Verify that all worked: `ls`
4. If you'd like, you can rename the director to "dockerapps" or something else more manageable: `sudo mv -r RockPro64-Simple-Server dockerapps`

## Required Apps
These applications will allow your server to run smoothly and securely. I recommend you start with these.

For *Traefik*, *fail2ban*, *Gotify*, and *Watchtower*, read through the *BaptisteBdn guide* referenced above to learn about those programs, then click on the links below to access my `.yml` files.

*Duplicati* is a program I found as a replacement for BaptisteBdn's *Borg*. Borg won't run on a RockPro64 as I couldn't find an arm64 version.

* [Traefik](traefik/)
* [fail2ban](fail2ban/)
* [Gotify](gotify/)
* [Watchtower](watchtower/)
* [Duplicati](duplicati/)

In general, here are the steps to modifying the files and executing them with Docker. I'll use Duplicati as the example:

1. Save the *.env* and *.yml* files into the directory */raid/dockerapps/duplicati*.
2. Navigate to that directory: `cd /raid/dockerapps/duplicati`
3. Modify the environment file to your network specs: `sudo nano .env`
4. Once you've made all your changes, write those changes with `Control-O` then exit with `Control-X`.
5. Open up the *.yml* file: `sudo nano docker-compose.yml`
6. Notice the *ports* specified (in this case 8200). You will need to forward any ports listed to your server from within your router's settings.
7. Notice the *volumes* specified. Those are actually *symbolic links*. Duplicati lives inside of a docker container and doesn't know of anything outside that container. So when it asks for the */backups* directory, Docker knows to give it access to */raid/duplicati-backups* which lives outside the container. The format of the symbolic link is exterior:interior. Create the exterior directories as necessary using the `mkdir` command.
8. Once you have modified and saved all files, you can call up Docker to get things rolling: `sudo docker-compose up -d`
9. Docker will begin downloading, extracting, and then starting. Once started, you can see all active containers with `sudo docker ps`.
10. To check the logs (a behind-the-scenes play-by-play which allows you to make sure everything is running smoothly and to troubleshoot): `sudo docker logs duplicati`
11. If you need to make any changes to the *.env* or *.yml* files, first stop the container with `sudo docker stop duplicati`, modify the files, then simply `sudo docker-compose up -d` to recreate the container with your updates.

Once you have Traefik, fail2ban, Gotify, Watchtower, and Duplicati running, then you can proceed with the fun stuff.


## Recommended Apps

For **Wordpress**, refer to the [BaptisteBdn guide](https://github.com/BaptisteBdn/docker-selfhosted-apps) then click on my link below.

For the others, the links below are all you need.

* [File Browser](filebrowser/)
* [Wordpress](wordpress/)
* [Flame](flame/)
* [ArchiveBox](archivebox/)
* [Syncthing](syncthing/)
* [Wiki.js]
* Pi-Hole - *Currently, I have Pi-Hole set up on a separate Raspberry Pi 3b. This program is likely my favorite of them all: it is a network-wide ad blocker that prevents ads from ever entering your network. This saves substantial speed and data usage. I am still looking into whether or not to move this to my RockPro64 server.*

**What I did and did not pull from the BaptistBdn Guide:**

| BaptistBdn App | arm64 Version? | Notes |
|:--|:--|:--|
| borg-backup | no | Replaced this with Duplicati. |
| fail2ban | yes | Copied over to this guide. |
| freshrss | ? | Did not use. |
| gotify | yes | Copied over to this guide. |
| jellyfin | ? | Did not use. |
| nextcloud | yes | Ultimately decided this had too many bells and whistles, ie, things to go wrong, so I simplified with FileBrowser |
| seafile | no | Replaced this with Syncthing. |
| synapse-element | ? | Did not use. |
| traefik | yes | Copied over to this guide. |
| transmission | ? | Did not use. |
| trilium | yes | I didn't like the UI so I opted for Wiki.js instead. |
| vaultwarden | ? | Did not use, but still considering. |
| watchtower | yes | Copied over to this guide. |
| webserver | yes | Copied over to this guide. |
| wireguard-pihole-unbound | ? | Did not use, but still considering. |
| wordpress | yes | Copied over to this guide. |

***

On to [Step 05: Backup Data and Power](05-backup-data-and-power.md)!
