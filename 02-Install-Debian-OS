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

RAID (Redundant Array of Inexpensive Disks) is a way of duplicating your data to protect against hardware failure; if one of your drives fail, you have a second that's an exact copy.

SSH into your server.

See all drives connected to your RockPro64:
`lsblk`

Your SSDs will likely show up as /dev/sda and /dev/sdb. This guide will continue under that assumption, but if you happen to have different disk names then use those instead.

Let's start with mounting and formatting the first disk:
`sudo fdisk /dev/sda`

Type "n" to create a new partition then select all the defaults.

Type "t" to select the partition type. We want to use "Linux RAID" or "Linux RAID autodetect" (whichever comes up for your particular drive). These may be listed with an identifier like "fd" or "29". You'll type that identifier into your command line prompt.

Type "w" to write the changes to the disk.

Confirm all changes were successfully made:
`disk -l`

Now do the exact same thing for your second disk (/dev/sdb).

## Step 3
Follow only a portion of this guide: https://unix.stackexchange.com/questions/544841/my-raid-1-always-renames-itself-to-dev-md127-after-rebooting-debian-10

Create the RAID (we will identify this new drive by /dev/md0):
`sudo mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sda /dev/sdb`

Edit the file "mdadm.conf":
`sudo nano /etc/mdadm/mdadm.conf`

Delete all content inside (if any) and replace it with this text (minus the ``` symbols):

```
# mdadm.conf
#
# Please refer to mdadm.conf(5) for information about this file.
#

DEVICE partitions

# auto-create devices with Debian standard permissions
CREATE owner=root group=disk mode=0660 auto=yes

# automatically tag new arrays as belonging to the local system
HOMEHOST <system>

# instruct the monitoring daemon where to send mail alerts
MAILADDR root

# definitions of existing MD arrays
```

Control-o to write-out and save changes.

Control-x to exit.


Now we need to add a reference to your new RAID inside the mdadm.conf file you just edited.

First, switch user to "root":
`sudo su - root`

Then:
`sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf`


Check that the changes were written to the file:
`nano /etc/mdadm/mdadm.conf`

The file mdadm.conf should look similar to this:

```
# mdadm.conf
#
# Please refer to mdadm.conf(5) for information about this file.
#

DEVICE partitions

# auto-create devices with Debian standard permissions
CREATE owner=root group=disk mode=0660 auto=yes

# automatically tag new arrays as belonging to the local system
HOMEHOST <system>

# instruct the monitoring daemon where to send mail alerts
MAILADDR root

# definitions of existing MD arrays
ARRAY /dev/md0 metadata=1.2 name=buster:1 UUID=1279dbd2:d0acbb4f:0b34e3e1:3de1b3af
```

If the command added something before the "ARRAY" line, delete it.

Exit the file then switch back to user "bob":
`su - bob`

Run `sudo update-initramfs -u`

Make sure you have "btrfs" files system on your OS:
`sudo apt install btrfs-progs -y`

Apply the btrfs file system to your RAID:
`mkfs.btrfs /dev/md0`

Finally, reference this guide for how to automount your RAID: [https://linoxide.com/raid-mirror-disks-in-linux/](https://linoxide.com/raid-mirror-disks-in-linux/)


Now our RAID is ready. But first, we have to mount it (ie, tell Debian we can use it to store data). We create a directory for mounting this device and create an entry in /etd/fstab file for automatically mounting on boot.

Make a new directory upon which we'll mount the RAID. This guide will use "/raid":
`mkdir /raid`

Mount our RAID disk md0 to the new directory /raid:
`mount /dev/md0 /raid`

Et voila! We can create and save files onto our RAID. Try it with this:
`touch /raid/file1.txt`


Now we need to modify /etc/fstab so that the RAID automatically mounts after a reboot:
`echo "/dev/md0 /raid btrfs defaults 0 0" >> /etc/fstab`

To see if that worked, unmount the RAID:
`umount /raid`

Then execute this and see if it remounted itself:
`mount -a`

The output of that command should show that /dev/md0 successfully mounted to directory /raid. Yay!


# Extra Credit

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
