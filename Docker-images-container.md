
# Imágenes
## Docker Hub
Es el repositorio central de docker para guardar imágenes públicas
Vamos a crear una cuenta es [docker hub](https://hub.docker.com/)

## Imágenes oficiales
En docker hub encontraremos las imágenes de los fabricantes que pueden ser utilizados de manera pública
cada image contine su **tag** que es un etiquetado de la versión de la imágen.
Se debe buscar la versión requerida e identificar su tag

## Descargar una imagen
`docker pull nginx`

### Tag de imágenes
`docker pull nginx:1.21.6`

###  Ver las imágenes descargadas en el docker host
`docker images`

### Documentación de las imágenes pública
Cada imágen publicada en el docker hub tiene la documentación necesario para crear y administrar el contenedor, el autor de la imagen es quíen administra la documentación de la página.
Se puede encontrar imágenes de los fabricantes para tambien de la comunidad open source

# Container o contenedor
Los contenedores son la ejecución de la imagen, una vez que tenemos la imagen descargada en el docker host podemos inicializar (ejecutar) y estaremos creado en container. La forma de ejecutar un contenedor es:
docker run <nombre_imagen>

## Crear un contenedor 
`docker run nginx`

## Crear un contenedor en background
`docker run -d nginx`

## Ver los contenedor ejecutando
`docker ps`
`docker ps -a`

## Crear un contenedor y darle un nombre
`docker run -d --name nginx-pruebas nginx`

## Crear un contenedor y exponer un puerto
`docker run --name nginx-pruebas -d -p 8080:80 nginx`

## Parar, iniciar y reiniciar un contenedor
`docker stop nginx-pruebas`

`docker start nginx-pruebas`

`docker restart nginx-pruebas`

## Eliminar contenedores
`docker rm -f nginx-pruebas`
la opción -f fuerza a eliminar un docker incluso si está ejecutando

Cuando veamos volumenes tendremos la opción  -fv para borrar el contenedor junto con el volumen atachado

`docker rm -fv nginx-pruebas`


# Recursos de un contenedor
El contenedor al ejecutarse en un docker host consummirá los recursos de este. Por lo que si no hay un control sobre los recursos podría afectar el rendimiento o la disponibilidad del sistema

## Ver los recursos que consume un docker

`docker stats nginx-pruebas`

## Limitar los recursos de un docker
`docker run --name nginx-pruebas -d -m "1500mb" -p 8080:80 nginx`

## Limitar el procesador
Necesitamos saber con cuantos cores dispone el docker hots para poder asignar al docker.
Esta información podemos sacar con estos comandos
`more /proc/cpuinfo`
`grep "model name" /proc/cpuinfo`

`docker run --name nginx-pruebas -d -m "1500mb" --cpuset-cpus 0-2 -p 8080:80 nginx`

