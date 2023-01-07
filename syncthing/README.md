# About

<p align="center">
<img src="/_utilities/syncthing.png" alt="syncthing" title="syncthing" />
</p>

Syncthing is a continuous file synchronization program. It synchronizes files between two or more computers in real time, safely protected from prying eyes. Your data is your data alone and you deserve to choose where it is stored, whether it is shared with some third party, and how it's transmitted over the internet.

* [Github](https://github.com/syncthing/)
* [Documentation](https://docs.syncthing.net/)
* [Docker Image](https://hub.docker.com/r/linuxserver/syncthing)

# Table of Contents

<!-- TOC -->

- [About](#about)
- [Table of Contents](#table-of-contents)
- [Files structure](#files-structure)
- [Information](#information)
    - [docker-compose](#docker-compose)
- [Usage](#usage)
    - [Requirements](#requirements)
    - [Configuration](#configuration)
- [Update](#update)
- [Security](#security)
- [Backup](#backup)

<!-- /TOC -->

# Files structure 

```bash
.
|-- .env
|-- docker-compose.yml
`-- config/
```

- `.env` - a file containing all the environment variables used in the docker-compose.yml
- `docker-compose.yml` - a docker-compose file, use to configure your application’s services
- `config/` - a directory used to store syncthing's configuration data.
- `config/Sync` - a directory used to store syncthing's default data folder.

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  version: "2.1"
  services:
    syncthing:
      image: lscr.io/linuxserver/syncthing:arm64v8-latest
      container_name: syncthing
      hostname: syncthing #optional
      environment:
        - PUID=${PUID}
        - PGID=${PGID}
        - TZ=${TIME_ZONE}
      volumes:
        - /raid/dockerapps/syncthing/config:/config
        # - /raid/dockerapps/syncthing/data1:/data1 # a directory of your choosing
      ports:
        - 8384:8384
        - 22000:22000/tcp
        - 22000:22000/udp
        - 21027:21027/udp
      restart: unless-stopped
      networks:
        - proxy
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.syncthing.rule=Host(`${TRAEFIK_SYNCTHING}`)"
        - "traefik.http.routers.syncthing.entrypoints=https"
        - "traefik.http.routers.syncthing.tls=true"
        - "traefik.http.routers.syncthing.tls.certresolver=mydnschallenge"
  # I commented out the following lines to save for reference. 
  # They're from the original docker-compose file from hub.docker.com.
  #      - "traefik.http.routers.syncthing.service=syncthing-svc"
  #      - "traefik.http.services.syncthing-svc.loadbalancer.server.port=8384"
  
  #      - "traefik.http.routers.syncthing.middlewares=auth"
  
  #      - "traefik.http.services.syncthing.loadbalancer.server.port=8384"
  #      - "traefik.http.services.syncthing.loadbalancer_sync.server.port=22000"
  #      - "traefik.http.services.syncthing.loadbalancer_sync_udp.server.port=21027/udp"
  #      - "traefik.http.middlewares.test-compress.compress=true"
  
  
        # Watchtower Update
        - "com.centurylinklabs.watchtower.enable=true"
  
  networks:
    proxy:
      external: true
  ```
* .env
  ```ini 
  TRAEFIK_SYNCTHING=sync.example.com
  TIME_ZONE=America/New_York
  PUID=1000
  PGID=1000    
  ```

# Usage

## Requirements
- [Traefik up and running](../traefik).
- A subdomain of your choice, this example uses `sync`.
    - You should be able to create a subdomain with your DNS provider, use a `A record` with the same IP address as your root domain.

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

You should then be able to access the syncthing web-ui at https://sync.example.com:8384. You will be prompted to set up a username and password on first login.


# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security
Brave Browser does not appear to like the https certificate; Safari is OK with it after you manually accept it.


# Backup

Docker volumes are globally backed up using [duplicati](../duplicati). 
