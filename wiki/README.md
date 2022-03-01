# About

<p align="center">
<img src="/_utilities/wikijs.png" alt="wikijs" title="wikijs" />
</p>

A modern, lightweight and powerful wiki app built on NodeJS

* [Github](https://github.com/Requarks/wiki)
* [Documentation](https://js.wiki/)
* [Docker Image](https://hub.docker.com/r/requarks/wiki)

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
- `data/` - a directory used to store wikijs's data

Please make sure that all the files and directories are present.

# Information

## docker-compose
Links to the following [docker-compose.yml](docker-compose.yml) and the corresponding [.env](.env).

* docker-compose.yml
  ```yaml
  version: "3"
  services:
    db:
      image: postgres:11-alpine
      environment:
        POSTGRES_DB: ${POSTGRES_DB}
        POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
        POSTGRES_USER: ${POSTGRES_USER}
      logging:
        driver: "none"
      restart: unless-stopped
      networks:
        - internal
      labels:
        - traefik.enable=false
      volumes:
        - /raid/dockerapps/wiki/data:/var/lib/postgresql/data
    wiki:
      image: requarks/wiki:latest
      depends_on:
        - db
      environment:
        DB_TYPE: ${DB_TYPE}
        DB_HOST: ${DB_HOST}
        DB_PORT: ${DB_PORT}
        DB_USER: ${DB_USER}
        DB_PASS: ${DB_PASS}
        DB_NAME: ${DB_NAME}
      restart: unless-stopped
      ports:
        - 3000
      labels:
       # - traefik.backend=wikijs
       # - traefik.frontend.rule=Host:${TRAEFIK_WIKI}
       # - traefik.docker.network=proxy
       # - traefik.port=3000
        - "traefik.enable=true"
        - "traefik.http.routers.wiki.rule=Host(`${TRAEFIK_WIKI}`)"
        - "traefik.http.routers.wiki.entrypoints=https"
        - "traefik.http.routers.wiki.tls=true"
        - "traefik.http.routers.wiki.tls.certresolver=mydnschallenge"
  
        # Watchtower Update
        - "com.centurylinklabs.watchtower.enable=true"
  
      networks:
        - internal
        - proxy
  volumes:
    db-data:
  networks:
    proxy:
      external: true
    internal:
      external: false
  
  ```
* .env
  ```ini 
  TRAEFIK_WIKI=wiki.example.com
  POSTGRES_DB: wiki
  POSTGRES_PASSWORD: typeYourPasswordHere
  POSTGRES_USER: wikijs
  DB_TYPE: postgres
  DB_HOST: db
  DB_PORT: 5432
  DB_USER: wikijs
  DB_PASS: typeYourPasswordHere
  DB_NAME: wiki   
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

You should then be able to access the wiki.js's web-ui and setup a new user account.


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