# Docker

## Cuenta en docker Hub
rugo0410/SAG+NAC-2

## Descargar Docker
En DockerHub existe un repositorio de imagenes

## Ejemplo básico
- Direcotorio de la app
dockerfile
/src
	index.php

[dockerfile]
FROM [distribuidor]:[paquete]
COPY [origen-app] [directorio-VM]
EXPOSE [puerto-VM]

FROM php:7.4.2-apache
COPY ./src /var/www/html
EXPOSE 80


- Crear la imagen 
```sh
# El "." es el directorio que contiene el dockerfile
docker build -t nombre-app . 
```

- Ejecutar la imagen 
```sh
# 8000 es el puerto local, 80 es el puerto de la VM
docker run -p 8000:80 nombre-app
```