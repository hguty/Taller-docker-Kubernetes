
# Docker registry
Es una aplicación de tipo servidor para almacenar y distribuir imágenes de docker en un repositorio local.
Se lo puede encontrar como imagen y es de código abierto

## Instalación de registry

`docker run -d -p 5000:5000 --name registry registry:2`

Usando un volumen:

~~~yaml
docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  -v /mnt/registry:/var/lib/registry \
  registry:2
~~~

## Tagear con el nuevo registry

`docker tag nginx:latest localhost:5000/nginx:blue`

##Subir la imagen al repositorio local

docker push localhost:5000/nginx:blue

# Un servidor Registry de la red local

Por defecto docker viene configurado para hacer un registro seguro (login),
para permitir un registry de red local debemos añadir la ip del servidor en la configuración de Docker

Modificar el archivo /etc/docker/daemon.json en los equipos que quieran hacer uso del repositorio local

*insecure-registries": ["127.0.0.1"]*

por

*insecure-registries": ["127.0.0.1", "<ip_server>:<port>"]*

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

## Docker registry con certificado autorfirmado

Crear un directorio para generar certificados.

`mkdir -p certs`

`cd certs`

Generar certificado digital y clave privada

~~~
openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key \
  -addext "subjectAltName=IP:<IP_Server_Adrress>" \
  -x509 -days 700 -out certs/domain.crt
~~~

Ejecutar docker run para correr el contenedor registry.  

~~~
docker run -d -p 5000:5000 --restart=always --name registry \
  -v /certs:/certs \
  -v /mnt/registry:/var/lib/registry \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
~~~

En el comando identificamos 2 volumenes, 1 para el certificado y key creados en el directorio cert y el otro volumen para almacenar las imágenes que guardará el registry.

Validamos la correcta ejecución del contenedor con `docker ps`.

## Configurar el Docker daemon qua acepte el certificado

copiar el archivo .crt a la ubicación de docker, 

en Linux: 

`sudo cp /certs/domain.crt /etc/docker/certs.d/IP_ADDRESS_OF_YOUR_VM:5000/ca.crt`

