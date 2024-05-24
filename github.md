Github
Desplegar un proyecto de angular en github pages
Fuentes:
https://dev.to/gedgonz/haciendo-deploy-de-una-app-en-angular-a-githubpages-4bll
https://www.npmjs.com/package/angular-cli-ghpages
https://angular.schule/blog/2020-01-everything-github

Crear un repositorio vacío en github
Crear el proyecto de angular y asignar el remote al repositorio creado
git remote add origin git@github.com:RgmDev/angular-github-pages.git

Agregar angular-cli-ghpages al proyecto de angular
ng add angular-cli-pages

Desplegar el proyecto 
ng deploy --base-href=/nombre-del-repo/

TROUBLESHOOTING

El proyecto no se despliega, muestra el error Timeout reached, aborting!.

Comprobar el estado de github: 
https://www.githubstatus.com/

Comprobar el tamaño total de los archivos: https://github.com/community/community/discussions/35197

El proyecto se despliega correctamente pero se ve en blanco, la consola del navegador indica el error Not allowed to load local resource en los archivos css y javascript.

Este problema se produce cuando se ejecuta el comando ng deploy en la consola git bash en windows

Si utilizas el repositorio principal de tu cuenta de github para montar la página.

Si al desplegar el proyecto obtienes por consola el error:
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
Cuando utilizas angular-cli-ghpages debes hacerlo con el flag --base-href=/