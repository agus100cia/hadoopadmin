# hadoopadmin

Hadoop Administrator

Este proceso pone a prueba tus conocimientos como Administrador de una plataforma de Hadoop.

El objetivo de esta tarea es analizar los archivos de log de un servidor

- 1.- Descarga el archivo de logs desde y cargalos al hdfs

```sh

wget http://hadooptutorial.info/wp-content/uploads/2014/11/common_access_log.txt
hdfs dfs -put common_access_log.txt /user/admin/logdata

``` 

- 2.- Crea una tabla de Hive con las funciones de SERDE 

```sql

CREATE TABLE apache_common_log (
  host STRING,
  identity STRING,
  user STRING,
  time STRING,
  request STRING,
  status STRING,
  size STRING
  )
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'
WITH SERDEPROPERTIES (
  "input.regex" = "([^ ]*) ([^ ]*) ([^ ]*) (-|\\[[^\\]]*\\]) ([^ \"]*|\"[^\"]*\") (-|[0-9]*) (-|[0-9]*)",
  "output.format.string" = "%1$s %2$s %3$s %4$s %5$s %6$s %7$s"
)
STORED AS TEXTFILE;

``` 

- 3.- Carga los datos del archivo a la tabla de hive

```sql

LOAD DATA LOCAL INPATH "/user/admin/logdata" INTO TABLE apache_common_log;

``` 

- 4.- Crea un snapshot de la carpeta hdfs /user/admin/logdata llamado snap0

```sh

hdfs dfsadmin -allowSnapshot /user/admin/logdata

hdfs dfs -createSnapshot /user/admin/logdata snap0

``` 

- 5.- Restaura los datos del snapshot en la carpeta del hdfs /user/admin/backup

```sh

hdfs dfs -cp -ptopax  /user/admin/logdata/.snapshot/snap0 /user/admin/backup

``` 


Resultado: Cargar a Classroom un pantallazo del contenido de la carpeta /user/admin/backup
