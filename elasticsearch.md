Elasticsearch
¿Qué es?
Es un motor de base de datos, funciona a través de su API y es muy rápido y potente. Trabaja como una base de datos no relacional indexada.
Instalación
Guías de instalación según el sistema operativo: 
https://www.elastic.co/guide/en/elasticsearch/reference/8.6/install-elasticsearch.html

Ejemplo de instalación en Redhat:

Fuente: 
https://computingforgeeks.com/install-elastic-stack-elk-on-rhel-centos/

Instalar Java:
yum -y install java-11-openjdk java-11-openjdk-devel

Configurar los límites de memoria de Java (Sobre todo dependiendo del entorno de ejecución)
En el archivo /etc/elasticsearch/jvm.options aplicarle las siguientes actualizaciones
-Xms256m
-Xmx512m

Agregar el repo de elasticsearch
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch

Configurar el repo añadiendo el siguiente contenido al archivo /etc/yumrepos.d/elasticsearch.repo
name=Elasticsearch repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md

Actualizar el índice de paquetes de yum
yum clean all
yum makecache

Instalar elasticsearch
yum -y install elasticsearch

Configuración de elasticsearch, archivo /etc/elasticsearch/elasticsearch.yml
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch

network.host: 0.0.0.0

xpack.security.enabled: false
xpack.security.enrollment.enabled: false
xpack.security.http.ssl:
  enabled: false
xpack.security.transport.ssl:
  enabled: false

Arrancar el servicio
systemctl start elasticsearch
systemctl enable elasticsearch

Probar la comunicación con elasticsearch
Ejecutaremos el comando:
curl -X GET localhost:9200

Esperando una respuesta de este tipo: 
{
  "name" : "localhost.localdomain",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "_yNoaSr4SeShhkRNDkDC6A",
  "version" : {
    "number" : "8.1.0",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "3700f7679f7d95e36da0b43762189bab189bc53a",
    "build_date" : "2022-03-03T14:20:00.690422633Z",
    "build_snapshot" : false,
    "lucene_version" : "9.0.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
Toma de contacto 
Un primera practica que consiste en ingresar datos en elasticsearch, crear sinonimos para realizar búsquedas más efectivas y obtener resultados.

Fuente:
https://www.youtube.com/watch?v=3pfEC91hruE&list=PLwH0tlWs8nkQj4_gwq867Wfqf6_FmdcKr

Creamos 3 archivos:

alta-indice.json
{
 "settings": {
   "analysis": {
     "filter": {
       "my_synonym": {
         "type": "synonym",
         "synonyms": [
           "telefono,smartphone",
           "smartphone,telefono"
         ]
       }
     },
     "analyzer": {
       "spanish": {
         "tokenizer": "standard",
         "filter": [
           "lowercase",
           "my_synonym"
         ]
       }
     }
   }
 }
}

alta-productos.json
{"index": {"_index": "articulos"}}
{"id": "001", "title": "iPhone X", "description": "telefono Apple"}
{"index": {"_index": "articulos"}}
{"id": "002", "title": "Samsung X", "description": "telefono japones"}
{"index": {"_index": "articulos"}}
{"id": "003", "title": "Huawei", "description": "telefono chino"}
{"index": {"_index": "articulos"}}
{"id": "004", "title": "Video VHS", "description": "tochana 1"}
{"index": {"_index": "articulos"}}
{"id": "005", "title": "Cinta de video VHS", "description": "pelicula para tochana 1"}
{"index": {"_index": "articulos"}}

busqueda.json
{
 "query": {
   "multi_match": {
     "query": "VHS",
     "fields": ["title", "description"],
     "analyzer": "spanish"
   }
 }
}

Realizamos las siguientes consultas a la API de elasticsearch:

Crear los índices 
curl -X PUT http://127.0.0.1:9200/articulos -H "Content-type: application/json" --data @alta-indice.json

Grabar los datos de productos
curl -X POST http://127.0.0.1:9200/_bulk -H "Content-type: application/json" --data-binary @alta-productos.json

Realizar la consulta
curl -X POST http://127.0.0.1:9200/articulos/_search -H "Content-type: application/json" --data @busqueda.json