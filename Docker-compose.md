# Docker compose
##Introducción
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
services:
volumes
network:
~~~
version y service son secciones obligatoorias
volumes y network son opcionales depende de su necesidad

- version con esta opción indicamos debemos indicar la versión que usaremos para docker-compose. Actualmente tenemos la versión 3.9
- services: Esta sección 