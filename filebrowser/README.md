# About

<p align="center">
<img src="/_utilities/filebrowser.png" alt="filebrowser" title="filebrowser" />
</p>

filebrowser provides a file managing interface within a specified directory and it can be used to upload, delete, preview, rename and edit your files. It allows the creation of multiple users and each user can have its own directory. It can be used as a standalone app or as a middleware.

* [Github](https://github.com/filebrowser/filebrowser)
* [Documentation](https://filebrowser.org/)
* [Docker Image](https://hub.docker.com/r/hurlenko/filebrowser)

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
- `data/` - a directory used to store filebrowser's data

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  version: "3"
  
  services:
    filebrowser:
      image: hurlenko/filebrowser
     # user: "bob:root"
      container_name: filebrowser
      ports:
        - 8080
      volumes:
        - /raid/dockerapps/filebrowser/data:/data
        - /raid/dockerapps/filebrowser:/config
      environment:
        - files.example.com=/filebrowser
      restart: unless-stopped
      networks:
        - proxy
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.filebrowser.rule=Host(`${TRAEFIK_FILEBROWSER}`)"
        - "traefik.http.routers.filebrowswer.entrypoints=https"
        - "traefik.http.routers.filebrowser.tls=true"
        - "traefik.http.routers.filebrowser.tls.certresolver=mydnschallenge"
       # - "traefik.http.routers.filebrowser.service=filebrowser"
       # - "traefik.http.services.filebrowser.loadbalancer.server.port=8080"
  
        # Watchtower Update
        - "com.centurylinklabs.watchtower.enable=true"
  
  networks:
    proxy:
      external: true
  ```
* .env
  ```ini 
  TRAEFIK_FILEBROWSER=files.example.com    
  ```

# Usage

## Requirements
- [Traefik up and running](../traefik).
- A subdomain of your choice, this example uses `files`.
    - You should be able to create a subdomain with your DNS provider, use a `A record` with the same IP address as your root domain.

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

You should then be able to access the filebrowser web-ui and setup a new user account.


# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security

While the webui utilizes https so data transmission is encrypted, take note that the files sit unencrypted on your server drives.


# Backup

Docker volumes are globally backed up using [duplicati](../duplicati). 