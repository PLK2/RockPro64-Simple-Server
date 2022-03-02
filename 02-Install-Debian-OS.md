# 02 - Install Debian OS and Setup RAID
*Set aside a couple hours.*

Before we get started, a few things:

- There's *graphical user interface* (GUI) and then there's *command line interface* (CLI). GUI is what you're already used to: Windows or macOS, where files look like files and trashcans look like a trashcans. But you realize there aren't real *files* in your computer, right? It's a facade to make it easier for us mere mortals to interact with computers. I read somewhere that **"GUI makes easy things easier, but CLI makes difficult things possible."** GUI is like teeth-whitening tooth paste: someone else has done the chemistry, you just need to dab it on. CLI, on the other hand, is like doing invasive oral surgery: you're drilling out your own teeth but the payoff is being able to install your own gold grill. It's an awesome power, but you have to be careful. Pay extreme attention to the smallest of details. I recommend copying/pasting the commands from this guide to minimize the chance of a typo.
- Speaking of Windows and macOS, forget about those and let's talk about Linux. **Linux** is the behind-the-scenes operating system that powers the world and we're going to use it to power our server. Since you're interested in getting into the server business, it's a good skill to have. As the joke goes:

> "Daddy, what are clouds made of?"
> 
> "Linux servers, mostly."

- All computer processors run on different *architectures*: there's x86, ARM, ARM64, etc. For our purposes, all you need to know is that the **RockPro64 has an ARM64 processor**. That means that any program needs to be built specifically for this architecture, otherwise it won't work.

Ok, let's get started.


## Step 1. Download Debian
*Allocate 5 minutes.*

> Note: Steps 1 and 2 are covered by [Pin64 here](https://wiki.pine64.org/index.php/ROCKPro64_Software_Release), but I'm going to walk through it with a bit more detail here.

**Debian** is the **operating system** we will use for our server. We're going to make it **headless** (meaning there's no desktop interface, only a command-line accessed from another computer via SSH), but to make the install process easier we're going to attach a temporary head to it (a monitor) and keyboard.

1. On your existing computer, go to Debian's download page:
[https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/netboot/SD-card-images/](https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/netboot/SD-card-images/)
2. Download and save these files to your *Downloads* folder:
  - `firmware.rockpro64-rk3399.img.gz`
  - `partition.img.gz`
3. Open Terminal on your Mac (or equivalent).
4. In Terminal, navigate to your Downloads folder: `cd Downloads`
5. We're going to **verify** the files just downloaded (to make sure they are complete and also not malicious). In Terminal within your Downloads directory, run: `shasum -a 256 firmware.rockpro64-rk3399.img.gz` then `shasum -a 256 partition.img.gz`.
6. Compare the outputs of both of those commands (a long string of numbers and letters) to the equivalent strings for each file listed in here: [https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/SHA256SUMS](https://deb.debian.org/debian/dists/bullseye/main/installer-arm64/current/images/SHA256SUMS) It should match exactly.
7. Back in Terminal, combine the downloaded files into a single disk image (an "image" is nerd-speak for "the file you double click on to install something"): `gzcat firmware.rockpro64-rk3399.img.gz partition.img.gz > complete_image.img`

Now that we have the Debian image, we're going to flash it (aka, burn it; aka, write it) to our microSD card.


## Step 2. Flash to microSD
*Allocate 5 minutes.*

1. On your existing computer, download, install, and open balenaEtcher: [https://www.balena.io/etcher/](https://www.balena.io/etcher/)
2. Insert the microSD card into your existing computer.
3. In balenaEtcher, click *Select image* and select your file `complete_image.img`.
4. Click *Select drive* and select your microSD card.
5. Click *Flash!*, input your password if prompted, then a few seconds later your microSD is ready.

Eject your microSD and insert it into your powered-off RockPro64.

## Step 3. Install Debian on the RockPro64
*Allocate 30 minutes*

Connect a monitor and keyboard to your RockPro64 (this is only temporary during the installation process) then plug in the power cord. You should see some little lights turn on on the board. *It's alive!*

The installation should automatically begin. You'll see things happening on your monitor. Wait a bit and eventually you'll be prompted for the following  input:

1. The **hostname** is how your server will be identified by your network router. For this guide, we'll use the hostname "superserver".
2. For *domain*, leave blank.
3. The installation process will set up two different *users*. The *root* user is all-powerful and has access to everything.  When prompted, create a password for this user.
4. The second user (for this guide we'll use username "bob") will need a separate password.
5. Once you arrive at the **Detecting disks / partitioning** portion of the installation, select your microSD (which will show up as mmcblk1 or something).
6. Select **Guided: Use the largest continuous free space**, then select your microSD.
7. You'll be asked *Write the changes to disk?* Confirm all looks good then select "yes" (use the tab key then hit return).
8. Next you'll be asked what exactly to install. Do not include a desktop or web server (select or unselect with the spacebar, and move up and down with the arrow keys) when prompted. **The only thing you need to select is SSH and standard utilities.**

After a while longer, you'll see a login prompt up on the monitor.

1. Type in "root" for your user, then type in your password (note that your password keystrokes will not show up on screen).
2. Install any updates: `apt update && apt upgrade`
3. Install sudo (don't worry about it): `apt install sudo`
4. Give user "bob" sudo privileges: `usermod -aG sudo bob`
5. Exit the session: `exit`

To access the server from another computer on your network, you'll need to add a **port forward** on your router. This process is different for each router, but for mine (an Asus) it goes like this:
1. Log into the router dashboard, which for me just means going to a web browser and typing in `http://192.168.10.1/` (the IP address of my router).
2. Navigate to *Advanced Settings / WAN / Port Forwarding*
3. Click *Add Profile*
4. Add a *Service Name* for reference, like "SuperServer22".
5. Set *Protocol* to TCP.
6. Set *External Port* to 22.
7. Then select the *Internal IP address* of your server.
8. Then click *Ok* and you're set.

Now open Terminal and SSH into your server as "bob": `ssh bob@superserver.local` (If "superserver.local" doesn't work, you can replace that with the IP address of your server). If SSH was successful, you can unplug the keyboard and monitor from the RockPro64. Congratulations: you have a headless server.

> Note: The root user does not have SSH enabled by default (this is for security purposes). For convenience, you'll want to enable that temporarily while getting everything set up. Then once you're done you can turn it off again. Follow these steps:
> 1. With your keyboard and monitor still plugged into your RockPro64, login as "root" user.
> 2. Execute: `nano /etc/ssh/sshd_config`
> 3. Alter the line "#PermitRootLogin" by removing the "#" and changing the option to "yes".
> 4. Write the changes then exit: `Control-O` then `Control-X`
> 5. Apply the changes: `systemctl restart sshd`
> 6. Now from another computer see if you can SSH into your server as root.


## Step 4. Mount External Drives and Set Up RAID1
*Allocate 30 minutes*

**RAID (Redundant Array of Inexpensive Disks)** is a way of duplicating your data to protect against hardware failure; if one of your drives fails, you have a second that's an exact copy. Since we're ditching Google and its convenient backup systems, that means we need to make our own backups. Skipping the middle (wo)man means you're THE (WO)MAN. It's all on you.

1. SSH into your server as user "bob": `ssh bob@superserver.local`
2. See all drives connected to your RockPro64: `lsblk`
3. Your SATA drives will show up as `/dev/sda` and `/dev/sdb`, or something similar. This guide will continue under that assumption, but if you happen to have different disk names then use those instead.
4. We'll start with mounting and formatting the first disk: `sudo fdisk /dev/sda`
  - Type "n" to create a new partition then select all the defaults.
  - Type "t" to select the partition type. We want to use *Linux RAID* or *Linux RAID autodetect* (whichever comes up for your particular drive). These may be listed with an identifier like *fd* or *29*. You'll type that identifier into your command line prompt.
  - Type "w" to write the changes to the disk.
5. Confirm all changes were successfully made: `disk -l`

Repeat those steps for your second disk `/dev/sdb`.

Now it's time to **set up the RAID**.
> Note: the following comes from a portion of this guide: [https://unix.stackexchange.com/questions/544841/my-raid-1-always-renames-itself-to-dev-md127-after-rebooting-debian-10](https://unix.stackexchange.com/questions/544841/my-raid-1-always-renames-itself-to-dev-md127-after-rebooting-debian-10)

1. Create the RAID (we will identify this new RAID drive as /dev/md0): `sudo mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sda /dev/sdb`
2. Edit the `mdadm.conf` file: `sudo nano /etc/mdadm/mdadm.conf`
3. Delete all content inside (if any) and replace it with this text:

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

4. Write-out and save changes: `Control-O`
5. Exit: `Control-X`


Now we need to **add a reference** to the new RAID drive inside the `mdadm.conf` file you just edited.

1. Switch user to "root": `sudo su - root`
2. Then as the root user, write the details of your RAID: `sudo mdadm --detail --scan >> /etc/mdadm/mdadm.conf`
3. Check that the changes were written to the file: `nano /etc/mdadm/mdadm.conf`

  - The file `mdadm.conf` should look similar to this:

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
ARRAY /dev/md0 metadata=1.2 name=buster:1 UUID=1309dda4:d0bcbb8g:0ce3434ce:21dbb2f33
```

  - If the command added something before the "ARRAY" line, delete it.

4. Exit the file then switch back to user "bob": `su - bob`
5. As bob, run: `sudo update-initramfs -u`
6. Make sure you have "btrfs" files system on your OS: `sudo apt install btrfs-progs -y`
7. Apply the btrfs file system to your RAID: `mkfs.btrfs /dev/md0`

Now the RAID is ready--almost. We have to **mount** it (ie, tell Debian we can use it to store data). We'll create a directory for mounting this device and create an entry in `/etd/fstab` file for automatically mounting on boot.

> Note: The following references this guide for how to mount and automount your RAID: [https://linoxide.com/raid-mirror-disks-in-linux/](https://linoxide.com/raid-mirror-disks-in-linux/)

1. Make a new directory upon which we'll mount the RAID. This we'll use */raid*: `mkdir /raid`
2. Mount the RAID disk *md0* to the new directory */raid*: `mount /dev/md0 /raid`
  - Et voila! We can create and save files onto our RAID. Try it with this: `touch /raid/file1.txt`
3. Modify the `/etc/fstab` file so that the RAID automatically mounts after a reboot: `echo "/dev/md0 /raid btrfs defaults 0 0" >> /etc/fstab`
  - To see if that worked, unmount the RAID: `umount /raid`
  - Then execute this and see if it remounted itself: `mount -a`
  - The output of that command should show that */dev/md0* successfully mounted to directory */raid*.

***

On to [Step 03: Install Docker](03-Install-Docker.md)!


# Extra Credit

## Install a Fan Controller
For some reason, the default setting is to have the fan running at full blast with no way to adjust it. A fan controller will automatically adjust the fan speed based on the temperature of your SBC.

Go to [https://github.com/tuxd3v/ats](https://github.com/tuxd3v/ats) and follow the installation instructions.

After install, you can see the status by executing: `systemctl status ats`


## Improve SSH Security
To SSH remotely (from outside the local home network), use this command in your terminal: `ssh -p 22 bob@YourPublicIPAddressHere`

(That means use port 22 connecting through the public IP address)

For better security, **change the default SSH port 22!** You can use any random number as long as it's below 64-thousand-something. Apparently, there are scary bots roaming the internet sniffing out unsuspecting port 22s and seeing if they can break in. By cutting off that port, you make it that much harder for a bot to find you. Here's how to do that:

1. SSH into your server as user bob the execute: `sudo nano /etc/ssh/sshd_config`
2. Find the line "#Port 22", remove the "#", then change the "22" to whatever number you want, like 11692: `Port 11692`
3. Write out, save, and exit.
4. Apply the changes: `sudo service sshd restart`

Now you can SSH in like so: `ssh -p 11692 bob@YourPublicIPAddressHere`

If you happen to have **fail2ban** already running (one of the programs we'll be installing), you need to follow these additional steps:
1. Navigate to the directory: `cd dockerapps/fail2ban/data/jail.d`
2. Edit the file: `sudo nano sshd.conf`
3. Find the line "Port = ssh" and change "ssh" to your chosen port number. Write out and exit.
4. Restart the fail2ban container: `sudo docker restart fail2ban`
