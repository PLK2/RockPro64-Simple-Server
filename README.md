# 02 - Install Debian OS
*Allocate a few hours for this.*

Before we get started, a few things:

- There's "graphical user interface" (GUI) and "command line interface" (CLI). GUI is what you're used to: Windows or macOS, where files look like files and trashcans look like a trashcans. But you realize there aren't real "files" in your computer, right? It's a facade to make it easier for us mere mortals to interact with computers. I read somewhere that "GUI makes easy things easier, but CLI makes difficult things possible." GUI is like teeth-whitening tooth paste: someone else has done the chemistry, you just need to dab it on. CLI, on the other hand, is like doing invasive oral surgery: you're drilling out your own teeth to install a gold grill. It's an awesome power, but you have to be careful. Pay extreme attention to the smallest of details. I recommend copying/pasting the commands from this guide to minimize the chance of a typo.
- All computer processors run on different *architectures*: there's x86, ARM, ARM64, etc. For our purposes, all you need to know is that the RockPro64 has an **ARM64 processor**. That means that any program needs to be built specifically for this architecture, otherwise it won't work.

Ok, let's get started.


## Step 1. Download Debian
*Allocate 5 minutes.*

Steps 1 and 2 are covered at the link below, but I'm going to walk through it a bit more here. [https://wiki.pine64.org/index.php/ROCKPro64_Software_Release](https://wiki.pine64.org/index.php/ROCKPro64_Software_Release)

Debian is the operating system we will use for our server.

Go to Debian's download page:
[https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/netboot/SD-card-images/](https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/netboot/SD-card-images/)

Download and save these files to your Downloads folder:
1. "firmware.rockpro64-rk3399.img.gz"
2. "partition.img.gz"

Open Terminal on your Mac.

In Terminal, navigate to your Downloads folder:
`cd Downloads`

In Terminal, combine the downloaded files into a single disk image:
`gzcat firmware.rockpro64-rk3399.img.gz partition.img.gz > complete_image.img`


Verifying a download does two things:
1. It ensures that you're downloading legitimate software from the developer and not something altered by bad actors.
2. It ensures that you actually downloaded every bite of information. Sometimes, errors can occur during the download process which can mess up your installation.


## Step 2. Flash to microSD
*Allocate 5 minutes.*

Download, install, and open balenaEtcher.
[https://www.balena.io/etcher/](https://www.balena.io/etcher/)

Insert your microSD card.

Click "Select image" and select your file "complete_image.img".

Click "Select drive" and select your microSD card.

Click "Flash!", input your password if prompted, then a few seconds later your microSD is ready.

Eject your microSD and insert it into your RockPro64.




## Step 3. Install Debian on the RockPro64
*Allocate 30 minutes*

Connect a monitor and keyboard to your RockPro64 (this is only temporary during the installation process) then power on.

The installation should automatically begin. You'll see things happening on your monitor. Wait a bit and eventually you'll be prompted for input.
1. The "hostname" is how your server will be identified by your network router. For this guide, we'll use the hostname "superserver".
2. For "domain", leave blank.
3. The installation process will set up two different "users". The "root" user is all-powerful and has access to everything.  When prompted, create a password for this user.
4. The second user (for this guide we'll use username "bob") will need a separate password.

Once you arrive at the "Detecting disks / partitioning" portion of the installation, select your microSD (which will show up as mlcmmd or something).:

Select "Guided: Use the largest continuous free space", select the SD //previously  I tried "Guided: Use entire disk" which ultimately failed, so maybe that was the problem?
Select "All files in one partition"

You'll be asked "Write the changes to disk?" Confirm all looks ok then select "yes" (use the tab key then hit return).

Next you'll be asked what exactly to install. Don't include a desktop or web server (select or unselect with the spacebar) when prompted. **The only thing you need to select is SSH and standard utilities.**

*Note: "root" user does not have SSH enabled by default. Leave it that way for safety, but to do it temporarily: [[202202011231 How to give root SSH access]]*

After a while longer you'll see a login prompt.

Type in "root" for your user, then type in your password (note that your password keystrokes will not show up on screen).

First things first, run this command to install any updates: 
`apt update && apt upgrade`

Then: 
`apt install sudo`

Give user "bob" sudo privileges. 
[[202201291714 Give a user sudo privileges]]

Exit the session:
`exit`

On another computer, open Terminal and SSH into your server in as "bob".

If everything worked, you can unplug the keyboard and monitor from the RockPro64. Congratulations: you now have a headless server.

## Step 4. Verify it works

"Daddy, what are clouds made of?"

"Linux servers, mostly."


## Step 5. Format External Drive(s) and Set Up RAID1
*Allocate 30 minutes*


# Extra Credit Work


## Install a Fan Controller
Install fan controller (otherwise the thing will blast unnecessarily at the highest RPM).

Go to [https://github.com/tuxd3v/ats](https://github.com/tuxd3v/ats) and follow the installation instructions.

After install, you can see the status:
`systemctl status ats`


## Improve SSH Security
To SSH remotely (from outside the local home network), use this command in terminal:

`ssh -p 22 pk@74.73.229.5
(that's port 22 connecting through the public IP address)

For better security, **change the default SSH port 22!**
[[202202232049 Change Default SSH Port 22 to Something Else for Better Security]]

Currently, I'm using port **46092**.
