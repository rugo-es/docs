# AWS Elastic Beanstalk

## Introducción 

Alojar una aplicación de nodejs en AWS utilizando el servicio de Elastic Beanstalk 

- Crear el entorno
  - Configuración del usuario y permisos del rol 
  https://stackoverflow.com/questions/77742666/aws-elastic-beanstalk-sample-app-not-able-to-use-role-to-obtain-required-permiss
  - Crea un zip con la aplicación para cargarlo en el entorno
    - Tener en cuenta que el despliegue que realiza Elastic Beanstalk, instalará las dependencias, no es necesario cargar la carpeta node_modules
  - Variables de entorno
    - Elastic Beanstalk carga las variables configuradas en el entorno dentro de la aplicación
    - La aplicación se ejecuta en un entorno nginx proxy reverse en el puerto 8080, no en el puerto 3000 como suele ser costumbre.