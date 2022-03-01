# About

<p align="center">
<img src="/_utilities/flame.png" alt="flame" title="flame" />
</p>

Flame is a self-hosted start page for your server. It is easy to setup and use. With built-in editors, it allows you to setup your own application hub in no time - no file editing necessary. I use it to manage not only the RockPro64 server but other devices on my network and various other resources.

* [Github](https://github.com/pawelmalak/flame)
* [Documentation](https://manual.seafile.com/docker/deploy_seafile_with_docker/)
* [Docker Image](https://hub.docker.com/r/seafileltd/seafile-mc)

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
- `data/` - a directory used to store flame's data

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  version: '2.1'

  services:
    flame:
      image: pawelmalak/flame:multiarch
      container_name: flame
      volumes:
        - "./data:/app/data"
      #  - /var/run/docker.sock:/var/run/docker.sock # optional but required for Docker integration feature
      ports:
        - 5005:5005
      environment:
        - PASSWORD=${FLAME_PASSWORD}
      restart: unless-stopped
      networks:
        - proxy
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.flame.rule=Host(`${TRAEFIK_FLAME}`)"
        - "traefik.http.routers.flame.entrypoints=https"
        - "traefik.http.routers.flame.tls=true"
        - "traefik.http.routers.flame.tls.certresolver=mydnschallenge"
        # Watchtower Update
        - "com.centurylinklabs.watchtower.enable=true"
  
  networks:
    proxy:
      external: true
  ```
* .env
  ```ini 
  TRAEFIK_FLAME=flame.example.com 
  PASSWORD=typeyourownpasswordhere     
  ```

# Usage

## Requirements
- [Traefik up and running](../traefik).
- A subdomain of your choice, this example uses `flame`.
    - You should be able to create a subdomain with your DNS provider, use a `A record` with the same IP address as your root domain.

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

You should then be able to access the flame web-ui with the FLAME_PASSWORD.


# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security



# Backup

Docker volumes are globally backed up using [duplicati](../duplicati). 