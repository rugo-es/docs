# Comandos Linux

- CHOWN 
Modificar el propietario de un directorio o fichero 

chown [-R] root /var/home 

- FIND

Busca ficheros (recursive default)
find directorio -name nombre-fichero

```sh
find . -name "fichero.txt" // Busca fichero llamados fichero.txt
```


- GREP 
Busca en el contenido de los ficheros

grep -r (recursive) -s (No muestra mensajes de Permission denied) cadena-a-buscar patron-ficheros

```sh
grep -r -s href *.php // Para buscar enlaces en los ficheros php
```


- TAR 

Comprimir y descomprimir archivos
```
tar -pczf fichero.tar.gz /directorio/a/comprimir/ (Comprimir)

tar -xzvf api-europcar-20191002.tar.gz --directory /c/xampp/htdocs/proyectos/destino/ (Descomprimir)
```



-SCP 

Secure copy, realiza copias de archivos a través de ssh 

scp usuario@host:/directorio/fichero.txt ./directorio/local/
```sh
scp usuario@151.80.59.119:/root/api-europcar-20191002.tar.gz .

scp -i  ~/.ssh/admiralfleet.pem base_backend.war ubuntu@52.31.205.50:/home/ubuntu/tmp/
```

- BACKUP PARA API DE EUROPCAR
tar -pczf api-europcar-20191212.tar.gz.tar.gz .
scp root@151.80.59.119:/var/www/html/europcar.fototasacion.com/api/api-europcar-20191212.tar.gz .


scp -i  ~/.ssh/admiralfleet.pem base_backend.war ubuntu@52.31.205.50:/home/ubuntu/tmp/

- Instalación de Composer en Linux
```sh
sudo apt update
sudo apt install curl php-cli php-mbstring git unzip

cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php

# HASH Actualizado (https://composer.github.io/pubkeys.html)
HASH=544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061

# Comporbar el HASH 
php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

# Ejecutar el instalador 
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Comprobar la instalación 
composer 
```