README

Definición de campos generales para todas las tablas

id mediumint unsigned primary key auto_increment,
created_at timestamp default current_timestamp,
updated_at timestamp default current_timestamp on update current_timestamp

TRIGGERS

Mostrar triggers 
SHOW TRIGGERS; 

Crear un trigger, ejemplo que actualiza un campo utilizando el id
DELIMITER $$
CREATE TRIGGER nombre_trigger
BEFORE INSERT ON nombre_tabla
FOR EACH ROW
BEGIN
	SET NEW.executionId = IFNULL( (SELECT MAX(executionId) + 1 FROM npombre_tabla), 4000820000001);
END$$
DELIMITER ;


Borrar un trigger 
DROP TRIGGER IF EXISTS nombre_trigger;



# MySQL 

https://www.anerbarrena.com/alter-table-mysql-5050/



## CREAR TABLA CON REFERENCIAS 
```sql
drop table if exists usuario_entidades;

create table if not exists usuario_entidades(
  id bigint(20) not null auto_increment,
  id_usuario bigint(20) not null, 
  id_entidad bigint(20) not null,
  primary key (id)
) auto_increment=1; 

alter table usuario_entidades add foreign key (id_usuario) references usuario (id);
alter table usuario_entidades add foreign key (id_entidad) references entidades (id);

desc usuario_entidades;
```


## AÑADIR UNA COLUMNA A UAN TABLA
```sql
ALTER TABLE usuario ADD COLUMN imagen VARCHAR(255);
```




## Create user accounts
```sql
CREATE USER 'user'@'server' IDENTIFIED BY 'pass';
```

## Delete user accounts
```sql
DROP USER 'user'@'server';
```

## Assign provileges and roles
```sql 
GRANT [privileges] ON [database].[table_name] TO 'user'@'server';
```

## Revoke privileges and roles 
```sql 
REVOKE [privileges] ON [database].[table_name] TO 'user'@'server';
```

## Create views
```sql 
CREATE VIEW [database].[view_name] AS 
SELECT * FROM [database].[table_name]
```

## Drop views 

```sql 
DROP VIEW IF EXISTS [database].[view_name] 
```

- Crear usuario 
CREATE USER 'username'@'%' IDENTIFIED BY 'excelia123';

 - Eliminar usuario completamente 
 DROP USER 'username'

 - Eliminar un usuario de una instancia concreta
 DROP USER 'username'@'host-1', 'username'@'host-2';  -- Para eliminar de varios host


- Dar permisos 
GRANT SELECT ON database.* TO 'username'@'%'


- Crear una vista 
CREATE VIEW vista AS [CONSULTA] 



# Mysql Tips 
- Group contact 
Agrupa en una sola celda los valores repetidos de una unión entre tablas
```sql
SELECT 
	t1.work_shops_status_id,
	t1.work_shops_id, 
    t1.work_shops_name, 
    t1.work_shops_address,
    t1.work_shops_city,
    t1.work_shops_postal_code,
    GROUP_CONCAT( t3.work_shops_types_public_name ORDER BY t2.work_shops_to_work_shops_types_order ASC SEPARATOR ', ') servicios_name
FROM work_shops t1 
LEFT JOIN work_shops_to_work_shops_types t2 ON t1.work_shops_id = t2.work_shops_id 
LEFT JOIN work_shops_types t3 ON t2.work_shops_types_id = t3.work_shops_types_id
GROUP BY
	t1.work_shops_status_id,
	t1.work_shops_id, 
    t1.work_shops_name, 
    t1.work_shops_address,
    t1.work_shops_city,
    t1.work_shops_postal_code
ORDER BY t1.work_shops_name ASC
```


## Buscar nombres de columna según tabla
USE information_schema;
SELECT * FROM COLUMNS WHERE TABLE_SCHEMA = '<Base de datos>' AND TABLE_NAME = '<Nombre de la tabla>';


## desinstalar completamente mysql 
Si haces un
```sh
apt-get remove –purge mysql-server
```

esperas que se desinstale completamente mysql de tu equipo. Pues no. Lo comprobarás si al cabo del tiempo intentas instalar otra versión del mismo o si decides pasarte a MariaDB. Te va a pedir la contraseña del root y hay de ti como no te acuerdes o, imagina que te acuerdas, se restaurarán todos los datos que tenías por ahí desperdigados y que creías que se habían borrado.

Para desinstalar completamente sigue los siguientes pasos:
```sh
sudo -i

service mysql stop

killall -KILL mysql mysqld_safe mysqld

apt-get --yes purge mysql-server mysql-client

apt-get --yes autoremove --purge

apt-get autoclean

deluser --remove-home mysql

delgroup mysql

rm -rf /etc/apparmor.d/abstractions/mysql /etc/apparmor.d/cache/usr.sbin.mysqld /etc/mysql /var/lib/mysql /var/log/mysql* /var/log/upstart/mysql.log* /var/run/mysqld

updatedb
```
Desinstalamos, borramos datos, eliminamos usuario y grupo.

Y si quieres borrar el log:
```sh
rm ~/.mysql_history
```


## No puedo conectarme desde un equipo externo 

- Puertos
- Configuracion del usuario (%)
- Configuracion MySQL

By default MySQL will listen for connections only from the local host. To enable remote connections like the one used by mysqlworkbench you will need to modify your /etc/my.cnf and change
```sh
bind-address 127.0.0.1
``` 
to
```sh
bind-address 0.0.0.0
```
or simply comment out the line completely. Once this is done, restart MySQL with the command
```sh
service mysql restart
``` 
and you should be able to make your connection.
