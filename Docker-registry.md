# Docker registry
Es una aplicación de tipo servidor para almacenar y distribuir imágenes de docker en un repositorio local.
Se lo puede encontrar como imagen y es de código abierto

## Instalación de registry

`docker run -d -p 5000:5000 --name registry registry:2`

## Tagear con el nuevo registry 
`docker tag nginx:latest localhost:5000/nginx:blue`

##Subir la imagen al repositorio local

docker push localhost:5000/nginx:blue

# Un servidor Registry de la red local
Por defecto docker viene configurado para hacer un registro seguro (login),
para permitir un registry de red local debemos añadir la ip del servidor en la configuración de Docker

Modificar el archivo /etc/docker/daemon.json en los equipos que quieran hacer uso del repositorio local

insecure-registries": ["127.0.0.1"]
por
insecure-registries": ["127.0.0.1", "<ip_server>:<port>"]

en otras distribuciones el archivo de configuración lo pueden encontrar en /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd 
añadir 
ExecStart=/usr/bin/dockerd --insecure-registry <ip>:<port>

Finalmente reiniciar el dockerd

`sudo kill -SIGHUP $(pidof dockerd)`
  
## Tagear con el nuevo registry 
`docker tag nginx:latest <ip_server>:5000/nginx:blue`

## Subir la imagen al repositorio local
docker push <ip_server>:5000/nginx:blue
