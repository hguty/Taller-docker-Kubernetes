## Networking
La conectividad de los contenedores es una parte fundamental para su funcionamiento. Su abstracción de las redes lo hace sencillo y muy poderoso. 
Los contenedores y los servicios no requieren saber que están ejecutandose en docker ni contra que otros servidores o clientes deben comunicarse, unicamente necesitan un tipo de red y una ip para funcionar.   

##Funcionamiento de la red en los contenedores de docker
Cuando instalamos docker en el sistema operativo, se cren una nueva interface de red (con algún direccionamiento).
En una terminal digitemos: `ifconfig` o `ip a`
Veremos una interface con nombre docker, todos los conetenedores creados  por defecto usan esta red para comunicarse con el host. 
`docker run -d nginx`
`docker inspect nginx`
Validar los parametros de red del contenedor y comparar el direccionamiento ip con la interface de red del SO

Cuando creo un contenedor por defecto se crean con el tipo de red Bridge
Todos los conetenedores creados con el tipo bridge se va a poder ver entre si y al host a través de la interface de docker
##Nota importante: En la red de tipo bridge se pueden ver con la ip mas no con el nombre 
`docker network ls` ejecutar las redes existentes
`docker network inspect bridge` mostrará las propiedades de la red bridge


## Tipos de redes (Network drives)
En Docker vamos a encontrar algunos drivers o tipos de redes:

- bridge, Es una red privada interna creada por el docker host, es la red por defecto que traer todos los dockers. Se pueden comunicar entre contenedores y con el docker host. Cuando queremos alcanzar a este host desde fuera del docker host tendré que hacerlo publicando puertos y mapenadolos con los puertos del docker host.
- host. El contenedor usa la configuración de red del docker host. 
- ipvlan. Este driver da un control completo del direccionaniento IP, tanto IPv4 como IPV6. El contenedor puede obtener el direccionamiento de nuestra red local. Es decir el DHCP de la compañía le entrega la IP.
- mscvlan. es muy similar a ipvlan con la diferencia que cada cotenedor se le asigna una mac adres diferente.
- overlay. Se usa para cluster de swarm donde se requiere que cada nodo del cluster puedan verse y administrar la alta disponibilidad.
- none. No tiene conexión de red
  
## Creación de redes
#Comando para crear una red con un nombre y el driver por defecto: 
`docker network create test_red`

# Comando para crear una red con un nombre y especifico el tipo de red o driver necesario: 
`docker network create -d bridge --subnet 10.11.0.0/24 --gateway 10.11.0.1 test_red`

# Comando para ejecutar un contenedor en la red creada

Crear 3 contenedores, 2 de ellos a una red creada y el otro a una segunda red creada
`docker run --network test_red -d --name test1 -it centos`

`docker run -dit ubuntu /bin/bash`
`
`docker run --network mired -d --name linux2 -it ubuntu /bin/bash`


# Ejecutar pruebas de conectividad entre contenedores de la misma red, con contenedores de otra red, docker 
`docker exec linux2 bash -c "ping  <ip o host>"`
`docker exec centos bash -c "ping  <ip o host>` 
Nota, cuando creamos una red se puede hacer ping usando el nombre del host a mas de la ip

# Comando para conectar un contenedor creado a una red creada
`docker network connect <nombre_red> <nombre del host>`
Pueden practicar creando un contenedor sin especificar la red y luego adjuntandola a una red
con docker network disconnect se lo puede desconectar
`docker network disconnect <nombre_red> <nombre del host>`

## Comando para remover una red
`docker network rm test_red`
pero para proceder con la eliminación no debe haber ningún contenedor pegado a esa red
Eliminemos las redes creada 

## Dar un ip estática a un contenedor
al crear el contenedor y despues de especificar la red con la opción --ip <ip> se puede asignar estáticamente su direccionamiento
`docker run --network test_red --ip <ip_estatica> -d --name test1 -it centos`



# PComando para ver las opciones usamos la opción --help 
`docker network create --help`

## Crear un contenedor con el tipo de red host
`docker run --network host  -d --name test1 -it centos`

Validar que ip nos da con docker inspect y comparar cos la ip del docker host

Probar con las redes:
 - none
 - ipvlan
 - macvlan





`docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=Datos.123 -d mysql:latest`
