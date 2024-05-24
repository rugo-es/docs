# Apache

## Reescribir rutas según el método de la petición conservando el método y los datos enviados

Fuentes:

https://httpd.apache.org/docs/2.4/rewrite/flags.html#flag_qsa

https://stackoverflow.com/questions/358263/is-it-possible-to-redirect-post-data

https://htaccessbook.com/control-request-methods/

Se debe especificar la condición del método con RewriteCond y añadir el patrón que corresponda con la ruta que queremos reescribir, la ruta destino y los flags QSA (Permite que la redirección conserve el método y los datos de entrada) y L (Last, si el patrón coincide no seguirá validando más patrones).


Ej:
```sh
RewriteEngine On
RewriteCond %{REQUEST_METHOD} PUT
RewriteRule ^(.*)aggregation/(.*)$ /bancomer/enterpriseWS/api/executionIdPut/ [QSA,L]

RewriteCond %{REQUEST_METHOD} GET
RewriteRule ^(.*)aggregation/(.*)$ /bancomer/enterpriseWS/api/executionIdGet/index.php?executionId=$1 [QSA,L]
```
Otra manera de montarlo:
```sh
RewriteEngine On
RewriteCond %{REQUEST_METHOD} ^(PUT|GET)
RewriteRule ^(.*)aggregation/(.*)$ /bancomer/enterpriseWS/api/executionId/index.php?executionId=$2 [QSA,L]
```

