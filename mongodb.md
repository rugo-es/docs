DESCRIPCIÓN
Base de datos NoSQL basada en documentos
Trabaja muy bien con tecnologías javascript

Tiene un servicio en la nube: Atlas (probar)
https://www.mongodb.com/atlas

Tutoriales:
https://www.mongodbtutorial.org/



DESCARGA 
[Windows]
MongoDB Server
https://www.mongodb.com/try/download/community?tck=docs_server

MongoDB Shell
https://www.mongodb.com/try/download/shell?jmp=docs


INSTALACIÓN
[Windows]
Agregar al path la carpeta de instalacion de mongodb
Ej: C:\Program Files\MongoDB\Server\6.0\bin

Para instalar el comando mongosh, pasa el contenido de la descarga y agregalo a la carpeta de instalación de mongoDB server

MONGODB SHELL
Para iniciar la consola de mongodb ejecuta el comando mongosh en un terminal
Comandos básicos:

db
Devuelve la base de datos actual, por defecto test
show dbs
Muestra las bases de datos disponibles
use db_name
Genera una base de datos db_name y nos conecta a ella
show collections
Muestra las colecciones de la base de datos actual
db.mycollection.insertOne({“foo”: “bar”})
Crea la colección si no existe y crea el documento
db.mycollection.find()
db.mycollection.find().pretty()
Mostrar documentos de una colección
Visualización más amigable
> lex x = 10
> if (x == 10){
… print(‘ok’);
… }
ok
Comandos multilínea 
Interpreta javascript
exit
Terminar sesión de mongodb shell



Personalizar la shell 

> cmdCount = 1
> prompt = function ( ) {
… return (cmdCount++) + “> “;
… }


Muestra el número de comandos que se han ejecutado en la sesión

1> 
> host = db.serverStatus().host;
> prompt = function () {
… return db + “@” + host + “$ “;
… }


Muestra la base de datos seleccionada y el nombre del host

test@user$ 
> prompt = function (){
… return “Uptime:“ + db.serverStatus().uptime + “ Documents:” + db.stats().objects + “ > “;
}


Muestra el tiempo que lleva la sesión activa y el número de documentos almacenados

Uptime:12337 Documents:0





TOOLS
Compass - Gestor de base de datos
https://www.mongodb.com/products/compass



OPERACIONES BÁSICAS

Todas las operaciones sobre colecciones:
https://www.mongodb.com/docs/manual/reference/method/js-collection/

Operadores ($eq, $gt…)
https://www.mongodb.com/docs/manual/reference/operator/


Insert 

insertOne
Inserta un documento dentro de la colección indicada
db.collectionName.insertOne( {...} )
db.collectionName.insertOne( { “name”: “John Doe” } )

insertMany
Permite agregar varios documentos en la misma sentencia
db.collectionname.insertMany( [ {...}, {...} … ] )
db.collectionName.insertMany( [ {“name”: “John Doe”}, {“name”: “Ruben Gonzalez”} ] )


Query

find
db.collectionName.find( {...} )
db.collectionName.find( { name: “John” } )

regexp
https://www.mongodb.com/docs/manual/reference/operator/query/regex/#mongodb-query-op.-regex
Página para probar expresiones regulares
https://regexr.com/
db.collectionName.find( { name: /ruben/i } )

estadísticas de la consulta
Muestra información sobre la ejecución de la consulta 
db.collectionName.explain(“executionStats”).find()


Update 

updateOne
Se indican dos objetos, filtro (para indicar los registros que debe modificar) y update (datos a modificar)
Solo modificará el primer documento que coincida con el filter
updateOne( { filter }, { update } )
updateOne( { name: “John” }, { $set: { apellidos: “new lastname” } } )

updateMany
Modificará todos los documentos que coincidan con el filter
updateMany( { filter }, { update } )
updateMany( { name: “John” }, { $set: { apellidos: “new lastname” } } )


Delete

deleteOne
Solo eliminará el primer documento que coincida con el filter
deleteOne( { filter } )
deleteOne( { name: “John” } )

deleteMany
Eliminará todos los documentos que coincidan con el filter
deleteMany( { filter } )
deleteMany( { name: “John” } )




mongoimport 
Utilidad para importar datos a colecciones desde ficheros JSON

Formato standard, los objetos vienen sin delimitar por comas ni agrupar
{...} {...} {...}
mongoimport --db=dbName --collection=collectionName .\file.json


Formato array, el fichero debe seguir la siguiente estructura:
[ {...}, {...} … ]
mongoimport --db=dbName --collection=collectionName .\file.json --jsonArray


Index
Crear y administrar los índices de las colecciones

Crear índices
Se indica el campo que actuará de índice y el orden (1: ascendente, -1: descendente )
db.collectionName.createIndex( { field: value } )
db.collectionName.createIndex( { created_at: 1 } )

Obtener los índices de una colección
db.collectionName.getIndexes()

Eliminar índices 
db.collectionName.dropIndex(index_name)

Agregar filtros en el índice
db.collectionName.createindex(
  { created_at: 1 },
  { partialFiltrerExpresion: { created_at: { $gt: ‘2022-01-01’ } } }
)


Aggregate
Fichero javascript para interactuar con mongo con funciones de aggregate
Para ejecutar el fichero y ver el resultado, ejecutar el comando:
mongosh nombre_fichero.js

conn = new Mongo();
db = conn.getDB('test');


// Búsqueda simple
result = db.clientes.aggregate([
  { $match: { name: { $eq: "ruben" } } }
]);


// Búsqueda agrupando y ordenando
result = db.clientes.aggregate([
  { $group: { _id: "$name", total: { $sum: 1 } } },
  { $sort: { total: 1 } }
]);


// Filtrando los campos de salida
result = db.clientes.aggregate([
  { $match: { name: { $eq: "ruben" } } },
  // { $project: { _id: 0, name: 1 } }
]);


// printjson(result);
result.forEach(printjson);
