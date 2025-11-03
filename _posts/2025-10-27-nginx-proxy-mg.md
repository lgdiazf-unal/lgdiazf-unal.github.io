---
title: Nginx Proxy Manager
date: 2025-10-27 20:00:00 -5000
categories: [NETWORK, PROXY]
tags: [proxy,ssl]
---

Nginx Proxy Manager (NPM) es una herramienta de código abierto y gratuita que proporciona una interfaz web fácil de usar para gestionar proxies inversos basados en Nginx, facilitando la exposición segura de servicios web en una red.

Este post describe los pasos necesarios para poder publicar en servicio de manera segura utilizando el protocolo https con un certificado valido.

## Escenario

Se desplegó el servicio de [minio]({% post_url 2025-10-27-minio %}) que publica su enterfaz en el puerto 9001 utilizando el protocolo http. Se desea exponer el servicio por el puerto 443 utilizando el protocolo https ( sin necesidad de modificar la configuracion de minio)



## Instalacion

Se depliega la solucion como un contenedor de docker utilizando la [documentacion oficial](https://nginxproxymanager.com/setup/). 

se utiliza la configuracion mas basica utilizando un archivo `docker-compose.yml` para describir la composicion de los contendores y un archivo `.env` para definir configuraciones del servicio:


***docker-compose.yml***. 
```yml
services:
  app:
    image: 'jc21/nginx-proxy-manager:${PROXY_VERSION}'
    restart: unless-stopped
    ports:
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
    environment:
      - TZ=${TIME_ZONE}
    volumes:
      - ${DATA_PATH}:/data
      - ${LETSENCRYPT_PATH}:/etc/letsencrypt
```

***.env***
```conf
PROXY_VERSION=2.12.6
TIME_ZONE=America/<Zona>
DATA_PATH=<Path para el almacen de los datos>
# la carpeta de letsencrypt debe tener permisos 600
LETSENCRYPT_PATH=<Path>
```

**nota:** ya que la carpeta de letsencryp debe tener permisos 600 antes de desplegar la solucion se debe ejecutar el siguiente comando:
```bash
sudo chmod 600 <path_letsencrypt>
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

## Referencias de configuracion

Para configurar y exponer el servicio se ejecutará el procedimiento [Quick and Easy Local SSL Certificates for Your Homelab!](https://www.youtube.com/watch?v=qlcVx-k-02E)