Docker 
Índice

Tutoriales
Crear una red de equipos conectados con IP’s fijas


Tutoriales 

Learn docker with example, freecodecamp
https://www.freecodecamp.org/news/what-is-docker-learn-how-to-use-containers-with-examples/

PHP SLIM 4
https://dev.to/cherif_b/using-docker-for-slim-4-application-development-environment-1opm
Crear una red de equipos conectados con IP’s fijas
Fuentes:
https://stackoverflow.com/questions/27937185/assign-static-ip-to-docker-container

Crear la red virtual en docker, utilizar un rango de Ips fuera de la red por defecto docker (172.17.0.0/16)

docker network create –subnet=172.18.0.0/16 nombre_red

Al crear los contenedores asignar la red creada y una IP con los flag --net y --ip
Por ejemplo:

docker run --net nombre_red --ip 172.18.0.2 -it ubuntu bash  