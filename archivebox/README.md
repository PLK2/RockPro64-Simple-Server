# About

<p align="center">
<img src="/_utilities/archivebox.png" alt="archivebox" title="archivebox" />
</p>

ArchiveBox is a powerful, self-hosted internet archiving solution to collect, save, and view sites you want to preserve offline.

* [Github](https://github.com/ArchiveBox/ArchiveBox)
* [Documentation](https://archivebox.io/)
* [Docker Image](https://hub.docker.com/r/archivebox/archivebox)

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
- `data/` - a directory used to store archivebox's data

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  # Usage:
  #     docker-compose run archivebox init --setup
  #     docker-compose up
  #     echo "https://example.com" | docker-compose run archivebox archivebox add
  #     docker-compose run archivebox add --depth=1 https://example.com/some/feed.rss
  #     docker-compose run archivebox config --set PUBLIC_INDEX=True
  #     docker-compose run archivebox help
  # Documentation:
  #     https://github.com/ArchiveBox/ArchiveBox/wiki/Docker#docker-compose
  
  version: '2.4'
  
  services:
      archivebox:
          # build: .                              # for developers working on archivebox
          image: ${DOCKER_IMAGE:-archivebox/archivebox:master}
          command: server --quick-init 0.0.0.0:8000
          restart: unless-stopped
          networks:
              - proxy
          ports:
              - 8000:8000
          environment:
              - ALLOWED_HOSTS=*                   # add any config options you want as env vars
              - MEDIA_MAX_SIZE=750m
              # - SEARCH_BACKEND_ENGINE=sonic     # uncomment these if you enable sonic below
              # - SEARCH_BACKEND_HOST_NAME=sonic
              # - SEARCH_BACKEND_PASSWORD=SecretPassword
          volumes:
              - ./data:/data
              # - ./archivebox:/app/archivebox    # for developers working on archivebox
          labels:
              - "traefik.enable=true"
              - "traefik.http.routers.archivebox.rule=Host(`${TRAEFIK_ARCHIVEBOX}`)"
              - "traefik.http.routers.archivebox.entrypoints=https"
              - "traefik.http.routers.archivebox.tls=true"
              - "traefik.http.routers.archivebox.tls.certresolver=mydnschallenge"
           # Watchtower Update
              - "com.centurylinklabs.watchtower.enable=true"
  
  networks:
    proxy:
      external: true
  ```
* .env
  ```ini 
  TRAEFIK_ARCHIVEBOX=archive.example.com    
  ```

# Usage

## Requirements
- [Traefik up and running](../traefik).
- A subdomain of your choice, this example uses `archive`.
    - You should be able to create a subdomain with your DNS provider, use a `A record` with the same IP address as your root domain.

## Configuration

Replace the environment variables in `.env` with your own, then run :

```bash
sudo docker-compose up -d
```

You should then be able to access the archivebox web-ui and setup a new user account.


# Update

The image is automatically updated with [watchtower](../watchtower) thanks to the following label :

```yaml
  # Watchtower Update
  - "com.centurylinklabs.watchtower.enable=true"
```

# Security

Don't be reckless.


# Backup

Docker volumes are globally backed up using [duplicati](../duplicati). 