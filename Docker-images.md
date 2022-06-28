# Dockerfile: Como crear nuestra propia imagen
Hemos visto como ejecutar imágenes ya creadas y administrar estos contenedores, ahora vamos a ver como podemos crear nuestras propias imágenes para nuestras aplicaciones.
Lo primero que debemos saber es que una imagen es un empaquetado de todos los componentes
que requiere un aplicación para correr (sistema operativo, dll, código fuente). 
Estos elementos dentro de la imagen la vamos construyendo por capas. La capa base será el sistema operativo! Siempre se construirá imágenes a partir de una imagen ya existente.
En docker las imágenes se construyen a partir de uno archivo de texto llamados DockerFile

El formato del archivo Dockerfile es:
INSTRUCCIÓN argumento
### Comentarios empiezan con #

Instrucciones más utilizadas:
FROM <imagen> (Es siempre la primera instrucción del dockerfile porque especifica a partir de que imagen voy a crear mi imagen)
RUN (Instrucciones que se puede ejecutar en la terminal, generalmente son comando de linux o de la imagen base)
COPY/ADD (Copiar archivo desde nuestra maquina a la imagen)
ENV (Definir variable de entorno a la imagen)
EXPOSE (Para exponer un puerto )
LABEL (Agregar etiquetas)
VOLUME (Manejo de volumenes) 
CMD (comando que inicia los servicios que vamos a exponer)
ENTRYPOINT 

## Estructura básica de un dockerfile

1era SECCIÓN -> La primera instrucción siempre será importar la imagen base sobre la cual construiremos nuestra personalización
FROM <IMAGEN>  

2da SECCIÓN -> En esta sección (la más amplia) haremos las modificaciones que necesitamos para nuestra imagen, instalaciones, variables de entorno, publicación de puertos, copiar ficheros, etc
RUN | COPY/ADD | ENV | LABEL | VOLUME

3era SECCIÓN -> el comando para ejecutar, la tarea, el proceso o servicio que correrá en el docker 
CMD <COMANDO> | ENTRYPOINT <COMANDO>



### Creando el nuestra primera imagen
Empezamos creando un archivo con nombre Dockerfile (doc de texto)

`nano Dockerfile`

Editamos en contenido del archivo:

~~~ 
FROM ubuntu
RUN apt update -y
RUN apt -y install nginx
~~~

##Construir la imagen a partir del Dockerfile
docker build es el comando para construir imágenes a partir de un docker file. 

`docker build .` 

El punto representa el directorio donde estoy y en donde está el DockerFile, puede estar un path o una url

`docker image ls`
`docker build -t mi_httpd:1.0 .`


Ejecutar el comando para correr un contenedor con la imagen creada

`docker run mi_httpd`

Y que pasó ? 
Nuestra imagen creada unicamente tiene una instalación de aplicación pero ningún comando en ejecución, la imagen no tiene un CMD (comando para ejecución de primer plano)
Por eso al intentar ejecutarla va a dar un exit. Esto es porque un contenedor no está pensado para correr solamente un Sistema operativo como la VM, un contendor está pensado para correr un tarea, un proceso o un servicio como: web server, aplication server, database, script, etc. Cuando la tarea se termina de ejecutar el contenedor termina su ejecución. Entonces: El contenedor existe tanto tiempo como el proceso interno vive.
Si el servicio web falla y se interrumpe entonces el contenedor hace un exit. 


## CMD
Entonces quien define el proceso o tarea que va a correr en contenedor? 
Eso se especifica en el docker file con la instrucción CMD. Un contenedor debe tener un CMD para arrancar en primer plano.
Formas de ejecutar CMD
- CMD comando arg1 arg2
- CMD ["comando", "arg1", "arg2"]


Editar el Dockerfile y añadir la siguiente linea
`CMD systemctl start nginx.service`





## Etiquetado de imágenes
Nos ayuda a dar un nombre y a versionar nuestra imágenes (control de versiones)

 `docker tag <nombre_actual_imagen> <muevo_tag>`

Es una buena práctica ir etiquetando las imágenes con latest para la última versión de nuestra aplicación. Así sabemos cuando está correiendo la última versión

## ENTRYPOINT
Su funcionamiento es similar a CMD, es decir me sirve para ejecutar un proceso, tarea o servicio en primer plano y que ejecute junto al contenedor. Con la diferencia es que espera un parámetro como argumento para completar su ejecución. Este parámetro puede ser ingresado junto a la linea de comandos cuando ejecuto el docker o en el Docker file usando CMD




Crear un Dockerfile a partir de la imagen de Ubuntu y mandar a dorimir por 10 segundos el contenedor
~~~
FROM ubuntu
CMD sleep 10
~~~

Modificar el Dockerfile para que mediante la linea de comando especificar la cantidad de segundos enviar a dormir el contenedor
~~~
FROM ubuntu
ENTRYPOINT ["sleep"]
~~~

Modificar el Dockerfile para que mediante la linea de comando especificar la cantidad de segundos enviar a dormir el contenedor pero que si no especifico la cantidad de segundos por defecto se configure en 10.

~~~
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
~~~

## COMANDO RUN 

RUN comando arg1 arg2
RUN ["comando", "arg1","arg2"]

La instrucción RUN ejecutará  comando y por cada una se crea una capa encima de la imagen actual. 
La imagen confirmada resultante se usará para el siguiente paso en el Dockerfile
Con el comando RUN ejecutamos una instrucción en el proceso de construcción de la imagen (No en la ejecución del contenedor). Esta diferencia hace que sea el comando para ir modificando la capa inicial e ir creando nuestra propia imagen.
Generalmente es usado para instalar software dentro a una imagen base

## COPY/ADD 

COPY \[--chown=<user>:<group>\] \\<src\\>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]

La instrucción COPY copia nuevos archivos  y directorios desde un fuente origen  al filesystem de la imagen en el path establecido en el parametro <dest>.

ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

La instrucción  ADD copia nuevos archivos (locales o remotos) y directorios desde un fuente origen o URL al filesystem de la imagen en el path establecido en el parametro <dest>.

La principal diferencia entre COPY y ADD es que esta última permite copiar desde una ubicación remota del archivo con su URL.

El uso mas frecuente es copiar librerías, código fuente o cualquier información necesaria que el contenedor lo tenga.

##ENV (Definir variable de entorno a la imagen)
La instrucción ENV setea variables de entorno <key> a un valor <value>, estas variables y sus valores podrán ser usadas en la siguientes instrucciones y podrán ser renombradas en cualquier momento  

Ejemplos: Se puede usar comilla, sin comillas 

`ENV MY_NAME="John Doe"`
`ENV MY_DOG=Rex\ The\ Dog`
`ENV MY_CAT=fluffy`

O en una sola linea setear  varias variables
`ENV MY_NAME="John Doe" MY_DOG=Rex\ The\ Dog  MY_CAT=fluffy`

Las variables de entorno seteadas con ENV persisten cuando el contenedor se ejecuta. Se puede ver el valor de estas variables usando un docker inspect y cambiar su valor en la ejecución con --env <KEY>=<VALUE> 
The environment variables set using ENV will persist when a container is run from the resulting image. You can view the values using docker inspect, and change them using docker run --env <key>=<value>.

Si se requiere una varible de entorno solo durante la construcción de la imagen se podría usar con la instrucción ARG

#ARG
ARG <name>[=<default value>]

La instrucción ARG define variables que los usuarios puede pasar como parámetros en el momento de ejecutar el docker build usando la opción --build-arg <varname>=<valor>

ARG usuario=jhony

`docker build --buil-arg usuario=juan .`

Si durante la construcción de la imagen no se especifica un valor a la variable usuario, por defecto usa el valos seteado en el Dockerfile


##WORKDIR
Establece el directorio de trabajo dentro de la imagen



#EXPOSE 
EXPOSE 8080/[PROTOCOL]

La instrucción EXPOSE especifica los puertos en los que va a esuchar el contenedor cuando se ejecute.
Por defecto se soncifura perto tcp pero se puede indicar explicitamente entre TCP/UDP

##LABEL 
LABEL <key>=valor <key>=<valor>
La instrucción LABEL se usa para añadir etiquetas/metadatos a la imagen del contenedor 
Ejemplos de uso

LABEL vendor="ACME Incorporated"
LABEL app="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."

##VOLUME (Manejo de volumenes)
Se puede agregar un volume anonimo desde la creación del dockerfile. VOLUME solo requiere un parámetro que es la ubicación del punto de montaje dentro del contenedor. En el docker host se crea el volume en la ubicación configurada por defecto para los volumenes, por defecto: /var/lib/docker/volumes/ 


## CMD
El comando CMD permite setear el comando por defecto y/o los parámetros que se ejecutaran cuando se corra el contenedor sin especificar ningún comando. Puede ser sobreescrito cuando se ejecute el contenedor desde la linea de comandos.


Una imagen debe tener un CMD para que el contenedor quede vivo despues de ejecutarlo
El objetivo principal de un CMD es proporcionar valores predeterminados para un contenedor en ejecución
Solo puede haber una instrucción CMD en un Dockerfile. Si se incluye más de un CMD, solo tendrá efecto el último CMD.


Otros comandos importantes

`docker history -H <imagen:tag>`

`docker image rm <id_imagen>`

