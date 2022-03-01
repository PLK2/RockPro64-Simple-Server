# About

<p align="center">
<img src="/_utilities/duplicati.png" alt="duplicati" title="duplicati" />
</p>

Duplicati is a free, open source, backup client that securely stores encrypted, incremental, compressed backups on cloud storage services and remote file servers. 

* [Github](https://github.com/linuxserver/docker-duplicati)
* [Documentation](https://www.duplicati.com/)
* [Docker Image](https://hub.docker.com/r/linuxserver/duplicati)

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
`-- data/
```

- `.env` - a file containing all the environment variables used in the docker-compose.yml
- `docker-compose.yml` - a docker-compose file, use to configure your application’s services
- `config/` - a symbolic link for storing duplicati's configuration data.
- `raid/` - a symbolic link pointing to the server's dockerapps for backup.
- `backups/` - a symbolic link pointing to the location for storing duplicati backups.

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  version: "2.1"
  services:
    duplicati:
      image: lscr.io/linuxserver/duplicati
      container_name: duplicati
      environment:
        - PUID=${PUID}
        - PGID=${PGID}
        - TZ=${TIME_ZONE}
        - CLI_ARGS= #optional
      volumes:
        - /raid/dockerapps/duplicati/config:/config
        - /raid/dockerapps:/raid
        - /raid/duplicati-backups:/backups
      ports:
        - 8200:8200
      restart: unless-stopped
      networks:
        - proxy
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.duplicati.rule=Host(`${TRAEFIK_DUPLICATI}`)"
        - "traefik.http.routers.duplicati.entrypoints=https"
        - "traefik.http.routers.duplicati.tls=true"
        - "traefik.http.routers.duplicati.tls.certresolver=mydnschallenge"
  
        # Watchtower Update
        - "com.centurylinklabs.watchtower.enable=true"
  
  networks:
    proxy:
      external: true

  ```
* .env
  ```ini 
  TRAEFIK_DUPLICATI=backup.example.com
  TIME_ZONE=America/New_York
  PUID=1000
  PGID=1000    
  ```

# Usage

## Requirements
- [Traefik up and running](../traefik).
- A subdomain of your choice, this example uses `backup`.
    - You should be able to create a subdomain with your DNS provider, use a `A record` with the same IP address as your root domain.

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

You should then be able to access the duplicati web-ui and setup a new user account.


# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security

You have the option of encrypting your backups. Do it.


# Backup

Docker volumes are globally backed up using [duplicati](../duplicati). 