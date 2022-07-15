# Docker compose
## Introducción
Es una herramienta de docker que nos ayuda a definir y crear aplicaciones multicontenedor.
Automatiza el proceso de creación de contenedores.

Con docker-compose tendremos:
- Aislar los ambientes para cada contenedor en un solo host
- Mantener la persistencia de los datos cuando un contenedor se inicie o reinicie
- Solo recrear con contenedores que se han cambiado


Con docker compose podemos unir todos los comando en un solo archivo

	- contenedores
	- imagernes
	- volumenes
	- redes	
	- etc

## Instalación:

Previa instalación del repositorio de Docker, ejecutar los comandos:

~~~
    sudo apt-get update
    sudo apt-get install docker-compose-plugin
~~~

## Usando Docker compose

El archivo debe tener extensión .yml puede tener cualquier nombre; pero de manera 
estandar se suele poner como docker-compose.yml

### Secciones del archivo docker-compose

~~~
version: 
services: (REQUERIDO)
volumes
network:
~~~
**version** DEPRECADO desde la versión 3.8
**service** son secciones obligatooria y representa una abstracción de un recurso computacional de docker que consiste en un conjunto de contenedores ejecutandose en el docker host de acuerdo a las especificaciones indicadas
**volumes y network** son opcionales depende de su necesidad

Veamos el primer ejemplo:
~~~
version: "3.0"
services:
   web:
      container_name: miweb1
      image: nginx:latest
      ports:
        - "80:80"
~~~
## Ejecución de docker compose
Una vez que hemos creado el archivo compose.yaml la ejecución es mediante el comando:
`docker compose up -d`
en versiones anteriores la instrucción era:
`docker-compose up -d`

Para detener los contenedores del docker compose usamos la instrucción:
`docker compose down`
### Variables de entorno
Para agregar variables de entorno a la ejecución de un contenedor usamos la instrucción *environment*

environment:
  - RACK_ENV=development
  - SHOW=true

o

environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:

Ejemplo de uso de variables de entorno en docker compose

version: "3.0"
services:
   db:
     container_name: base
     image: postgres
     environment:
        - POSTGRES_PASSWORD=example

### env_file
Se puede poner todas las variables de entorno en un solo archivo y desde el compose.yaml llamar a ese archivo con la instrucción env_file

env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/runtime_opts.env

## Volumenes
Crear un volumen nombrado es crear la sección volumes dentro del archivo compose (identado a la altura de version)
~~~
volumes:
   ejemplo_volume:
~~~   
Y luego llamado desde la sección services dentro del contenedor que desea hacer uso de el:

~~~
services:
   db:
      container_name: algo
      volumes:
        - ejemplo_volume=/var/www/html/
~~~

Ejemplo:

~~~
version: "3.0"
services:
   db:
     container_name: base
     image: postgres
     environment:
        POSTGRES_PASSWORD: example
     volumes:
       - data:/var/lib/postgresql/data
volumes:
  data:
~~~

## Volumen nombrado
En caso de querer especificar la ruta específica del docker host para el volumen se deberá indicar esta en la sección de servicios de la siguiente manera:

~~~
services:
   db:
      container_name: algo
      volumes:
        - /root/mi_ruta_local/=/var/www/html/
~~~

## Network
Se puede crear redes y atachar a los contenedores desde el mismo docker compose

En la sección networks crear la red:
~~~
networks:
  nueva-red
~~~  
Y en la sección de servicios atachamos la nueva red también con la instrucción networks:

~~~
services:
   web:
      container_name: algo
      networks:
        - nueva-red
~~~

Cuando varios servicios se atachan a la misma red, se las puede ver alcanzar mediante su nombre

## Reinicio de contenedores (restart)
Condiciones por las que un contenedor se va a reriniciar o iniciar automáticamente. Encontramos 4:
- no No se reinicia automáticamente docker. Por defecto es ka opción habilitada
- on-failure.  Reiniciará el contenedor si ha sufrido una falla y se detuvo.
- always. El contenedor se iniciará siempre independiente de cuál fue la causa de que se haya detenido. En caso de que se detenga manualmente el contenedor, este se reiniciará una vez el demonio de docker *dockerd* se reinicie.  
- unless-stopped. Similar a *always*, excepto que cuando el contenedor se detiene (manualmente o de otra manera), no se reinicia incluso después de que se reinicia el demonio Docker.

Para incluir esta política de reinicio en un contenedor habrá que agregar la instrucción `restart: always|on-failure|unless-stopped`

## depends_on
Sirve para indicar la dependencia entre servicios. Por ejemplo un servicio web depende una que previamente este arriba una base de datos

Ejemplo:

~~~
version: "3"
services:
  web:
    image: nginx
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
~~~

El comportamiento esperado es que cualdo se levante el servicio, automáticamente levante el servicio del cual dependa.
Cuando reinicie el docker host no se iniciará el servicio hasta que su predecesor se inicie.
Cuando se da de baja el compose primero detiene el servicio y luego su dependencia.


