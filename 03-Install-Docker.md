# 03 - Install Docker

<p align="center">
<img src="/_utilities/docker.png" alt="docker" title="docker" />
</p>

## What? And Why?
**Docker is a program that helps you manage and organize other programs.**

Docker allows you to set up system-wide and network-wide configurations once and then apply those to all your containerized apps. This saves you from the hassle of having to do the same thing repeatedly for each app. It also helps with consistency: setup and upgrades can be applied across the board.

Docker Compose provides an easy format for configuring each of your apps. A ".yml" text file contains all the necessary commands and specs for spinning up, updating, or upgrading the app.

You'll appreciate it more once you see it in action so let's get started.

## Install Docker Engine

*Allocate 15 minutes*

To install **Docker Engine**, follow the instructions at Docker's website. There are two sections to follow: "Setup the repository" and "Install Docker Engine." Be sure to install the "**Debian**" version.

[https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)

*Bonus Note: if copying/pasting from their website, the backslash \ implies a line break, so in the terminal you need to delete that and leave only a single space between the command elements. E.g., the following code from their website,*

```
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

*should appear in your terminal line as:*

`sudo apt-get install ca-certificates curl gnupg lsb-release`

## Install Docker Compose

*Allocate 10 minutes*

To install **Docker Compose**, follow the instructions at Docker's website for the **Linux** version: 
[https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

Be sure to update the release number shown in their first command, which on their website appears as:
`sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

I.E., you should change the "1.29.2" to their latest release, which as of this writing would be "v2.2.3" (and yes, **include the "v"**). Go to their GitHub page to see the latest release number: [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases)

***

On to [Step 04: Install the Applications](04-Install-Applications.md)!

