# Volumen
Los volúmenes nos permiten almacenar datos PERSISTENTES del contenedor
Se puede compartir entre varios contenedores
Es la mejor solución para almacenar datos 
## Crear un docker de mysql con volumen
Al crear un volumen debo saber en que directorio local de mi servidor (docker host) voy a almacenar los datos del docker y también la ubicación dentro del contenedor en donde necesito la persistencia
Para nuestro ejemplo crearemos un docker de mysql y los datos los almacenamos en el directorio local: /opt/mysql/datos
Leyendo la documentación del docker se que los datos de mysql se guardan en /var/lib/mysql

`docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=Datos.123 -d mysql:latest`

Conectarnos al docker my sql y ver tables y crear una tabla
`mysql -u root -h 127.0.0.1 -pDatos.123`

### Crear tablas
`create database prueba1`
`create database prueba2`

Eliminamos el docker y volvemos a crear. Consultamos las base de datos


Repetimmos el ejercicio pero ahora creanos el docker con volumen

-v <Directorio_local>:<path_contenedor>

Esta imagen se crea pero no hay datos persistentes

`docker run --name mysql -v /opt/mysql/datos:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Datos.123 -d mysql:latest`

# Volumes anonymous
Se puede dejar que docker seleccion la ubicación del volumen, solo debemos dar el parámetro del path dentro del contenedor
Este directorio se crea con un nombre randomico en el directorio por defecto de volumenes de docker

`docker run --name mysql -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=Datos.123 -d mysql:latest`

## Ver la información de los volumnes
`docker volume ls`

para eliminar el docker junto con el volumen:
`docker rm -fv <docker_id>`

## Volumen nombrado
Aca se specifica un nombre específico para el volumen y la ubicación local

### Crear un volumen nombrado
Creo un volumen con un nombre especifico y lo al crear el contenedor lo atacho con su nombre.
Este volumen, con el nombre, se crea en el directorio por defecto de volumenes de docker 
`docker volume create <nombre_volumen> `

Para atacharlo a un contenedor especificamos su nombre
`docker run --name mysql -v nombre_volumen:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=Datos.123 -d mysql:latest`

Eliminar el volumen
`docker volume rm <nombre_volumen>`

##La ubicación de los volumenes
Para identificar la ubicación de los volumenes anonymos y nombrados debemos ver el path root de docker
`docker info`

la podemos encontrar en /var/lib/docker/volume

## Volume Dangling
Volumenes danglig son volumenes desreferenciados, es decir que su contenedores dejaron de apuntar 
Para eliminar estos volumenes ejecutamos la siguiente instrucción:
### Visualizar volumenes dangling 
`docker volume ls -f dangling=true`

### Visualizar solo los ids de volumenes dangling
`docker volume ls -f dangling=true -q`

### Eliminar solo los solumenes danglig
`docker volume ls -f dangling=true -q | xargs docker volume rm`





