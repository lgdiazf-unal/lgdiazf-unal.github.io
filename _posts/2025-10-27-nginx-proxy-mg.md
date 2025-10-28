---
title: Nginx Proxy Manager
date: 2025-10-27 20:00:00 -5000
categories: [NETWORK, PROXY]
tags: [proxy,ssl]
---

Nginx Proxy Manager (NPM) es una herramienta de código abierto y gratuita que proporciona una interfaz web fácil de usar para gestionar proxies inversos basados en Nginx, facilitando la exposición segura de servicios web en una red.

## Instalacion

Se depliega la solucion como un contenedor de docker, creando el siguiente ***docker-compose.yml***

```yml
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    #environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
Para desplegarlo basta con ejecutar el siguiente comando:

```bash
docker compose up -d
```
Desde un navegador se debe entrar a <ip_seervidor:81>, se debe ingresar con las siguientes credenciales:

```conf
Email:    admin@example.com
Password: changeme
```