

¡¡¡ VER DOCUMENTO EN GOOGLE DRIVE /docs y docs/pixelpro !!!


# git / Github / Bitbucket
Sistemas de control de versiones 

# Tutorial git basic
https://www.diegocmartin.com/tutorial-git/

## Git Flow

## Tools & utils 

- Ver historial de commits 
```git 
git log // simple
git log --pretty=oneline  // Formato en una línea
git log --pretty=format:"%h %s" --graph  // Formato en modo gráfico
``` 


--Variables de configuracion 
```git
git config --global --list // Listar variables globales del sistema
git config --list // Ver lista de variables
git config --global user.name "Ruben Gonzalez"
git config --unset user.mail // Eliminar una variable de configuracion 
git config user.mail ruben.gonzalez@mail.com // Indicar una variable de configuracion
```

-- Staging area 
```git
git add . // Añadir todos los archivos al staging area
```


## Casos
### Me he movido de HEAD y quiero volverlo a unir a la rama MASTER
```git
// Crear una nueva rama 
git branch temp
git checkout temp

// Comprobar las diferencias de la nueva rama con el repo local y remoto
git diff master temp
git diff origin/master temp 

// Forzamos unir el contenido de la rama temp a la rama master y cambiamos a master
git branch -f master temp
git checkout master

// Eliminamos la rama temp
git branch -d temp

// Actualizamos el repositorio remoto
git push origin master
```

## Instalación 

Desargar git de la página oficial...

## Configuración del usuario

- Después de realizar la instalación, configuramos las variables globales que nos identifican

```sh
$ git config --global user.name "Rubén González"
$ git config --global user.email "ruben.gonzalez@mkdautomotive.com" 
```

- SSH keys 
Para poder trabajar con repositorios remotos sin tener que estar identificandonos cada vez que realizamos una acción contra dicho repositorio, debemos configurar las claves SSH.
Para generar una clave SSH ejecutamos el siguiente comando
```sh
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa_vagrant.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
0d:39:23:48:38:a6:99:25:23:eb:80:3d:fb:ef:a2:ff rgonzalez@ubuntu
The key's randomart image is:
+--[ RSA 2048]----+
| .. |
|+ =. . . |
|o@ .. . = |
|B o . = |
|o o S . |
| .. |
| . |
| o |
| .o.=E |
+-----------------+
```
Nos pedirá un fichero donde almacenar la clave, por defecto "~/.ssh/id_rsa", aunque podemos indicarle cualquier otro
y una contraseña, que por defecto, dejamos vacía
Esta acción nos genera dos ficheros
- id_rsa (clave privada)
- id_rsa.pub (clave pública)
Ahora lo que debemos hacer es copiar el contenido de la clave pública en nuestra configuración de usuario en Bitbucket (Bitbucket settings/Security/SSh keys)
De esta manera nos aseguramos de trabajar en nuestro entorno local identificados con nuestro usuario Bitbucket
Para realizar la comprobación de que todo esta correcto, ejecutamos el siguiente comando y vemos con que usuario estamos trabajando
```sh 
$ ssh -T git@bitbucket.org
logged in as ruben-gonzalez

You can use git or hg to connect to Bitbucket. Shell access is disabled

```

Se puede gestionar con que clave quieres trabajar dependiendo del alojamiento del repositorio remoto, podemos crear el archivo ~/.ssh/config con el siguiente contenido
Donde indicamos el host destino y la ruta a la clave
(*) la segunda línea ( IdentityFile ~/.ssh/id_rsa) tiene sangría de 1 espacio y es obligatorio
```
Host bitbucket.org
 IdentityFile ~/.ssh/id_rsa
```

Dentro de un repositorio en local y cuando queramos trabajar contra un repositorio remoto, debemos asegurarnos de que apuntamos a la URL correcta del proyecto.
Para ver a que repositorio remoto apunta nuestro repositorio local ejecutamos el siguiente comando 
```
$ git remote -v
```
Para añadir la URL a la que apunta nuestro repositorio local 
```
$ git remote add origin accountname@bitbucket.org/accountname/reponame.git
```
Para cambiar la URL a la que apunta 
```
$ git remote set-url origin accountname@bitbucket.org/accountname/reponame.git
```





## Clonar un repositorio remoto a local
Para clonar el repositorio basta con ejecutar
```
$ git clone git@bitbucket.org:ruben-gonzalez/repositorio.git
```
Con este comando descargamos el repositorio y todas sus ramas, aunque solo se verá la master
```
$ git branch
* master
```
Para ver todas las ramas
```
$ git branch -a
* master
develop
ft/feature
```
Podemos movernos a cualquiera de estas ramas sin problemas













En ocasiones el elevado tamaño de algún archivo puede generar error al ejecutar un push:
```sh
$ git push -u origin master
Enumerating objects: 38029, done.
Counting objects: 100% (38029/38029), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10790/10790), done.
client_loop: send disconnect: Connection reset by peerMiB/s
fatal: the remote end hung up unexpectedly MiB | 307.00 KiB/s
fatal: sha1 file '<stdout>' write error: Broken pipe
fatal: the remote end hung up unexpectedly
```
Para solventar este problema podemos indicar un tamaño de buffer más adecuado:
```sh
$ git config http.postBuffer 524288000 
```


## Workflows
### Crear un repositorio local y alojarlo en Github / Bitbucket
Una vez desarrollado el proyecto en local, nos situamos en la raiz de este y ejecutamos los siguientes comandos:
```sh
$ git init      
$ git add .         
$ git status        
```
Para configurar el repositorio en Github/Bitbucket
Creamos desde el portal web el repositorio 
Copiamos la URL del proyecto (ej, git@bitbucket.org:ruben-gonzalez/proyecto.git)
y ejecutamos los siguientes comandos:
```sh
$ git remote add origin git@bitbucket.org:ruben-gonzalez/proyecto.git
$ git commit -m "Mensaje"       
$ git push -u origin master 
```