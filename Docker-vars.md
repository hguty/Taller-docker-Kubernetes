# Como interactuar con los contenedores
## Acceder al shell de un contenedor
El comenado 'docker exec' ejecuta un proceso en un contenedor en ejecución
`docker exec -ti <nombre_docker>|<container_id> bash` 

Con este comando te encontraras dentro del SO de la contenedor

## Crear variable de entorno en la ejecución de un contenedor
Hay dos formas de crear ñas variables de entorno:
1. Al crear la imagen por medio del DockerFile 
2. Cuando creo el docker, en tiempo de ejecución
   Se lo crea de la siguiente manera:

   `docker run -d httpd --name apache -e "puerto=80" httpd` \

   `docker run -d httpd --name apache -e "puerto=80" -e "ambiente=pruebas" httpd` \

   Se puede usar una variable definida en el sistema operativo del docker host como parte de la variable de entorno del docker

   `export sistemao=linux` \
   `docker run -d httpd --name apache -e "puerto=80" -e sistemao httpd`

## Copiar archivos/ directorios al contenedor

podemos usar el comendo docker cp para copiar archivos desde nuestro docker host hacia el contenedor así:

docker cp <archivo> <nombre:docker>:<ruta>

Para nuestro ejemplo vamos a crear un archivo index.html en el docker host y lo copiamos a la ubicación del indez de apache

Crear un pequeño archivo html y guardarlo como index.html
Copiar el archivo guardado a la ubicacación del docker donde de ejecuta apache

`docker cp index.html apache:/usr/local/apache2/htdocs/index.html`

También podemos traernos (copiarnos) un archivo del contenedor al server

`docker cp apache:/var/log/dpkg.log /home/hector/`

la opción -a copia directorio

`docker cp -a apache:/usr/local/apache2/icons /home/hector/`

