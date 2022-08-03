# Namespace

Es un objeto que nos va a permitir organizar y agrupar los recursos que se van creando en k8s.

Un espacio de nombre es como un "cluster virtual", y cada recurso se puede crear en espacio de nombres con lo que podríamos empezar a aislar los espacio de nombre y dar permisos de acceso.

Los namespaces existente por defecto son:

- default. Es el namespace por defecto en donde se crean todos los objetos. Si no expecificamos un namespace diferente será en este espacio de nombre donde se crea.
- kube-system. Es el espacio donde se crean los objetos propios del sistema.

`kubectl get all --namespace kube-system`

`kubectl get pods -A` #Lista todos los objetos tipo pods de todos los namespace

Los nombres de los objetos son únicos por cada namespace, es decir en un mismo namespace no puede haber 2 pod's con el nombre nginx; pero si pueden repetirse nombres de objetos si esán en otro namespace.

## Que objetos van en un namespace

No todos los recursos van dentro de un namespace.

De los objetos que hemos visto los siguientes no van dentro de un namespace:

- namespaces
- nodes
- volumenes y storage

Para poder ver que comandos van en un namespace podemos hacer la siguiente consulta:

`kubectl api-resources`

`kubectl api-resources --namespaced=false`

## Crear un namespace

Desde la línea de comandos:

`kubectl create namespace espacio1`

Creación de un namespace mediante un manifest yaml

~~~yaml
apiVersion: v1
kind: Namespace
metadata:
   name: espace2

~~~

kubectl apply -f name.yaml

## Crear un objetos dentro de un namespace 

Cuando creamos los archivos de manifiesto de los objetos y se quiere indicar explicitamente el namespace donde se creará, se debe agregar dentro del espacio de metadata agregar el campo 

`namespace: nombre_namespace`

Así el archivo de manifiesto de un pod dentro del namespace *espacio1*, sería:

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
  namespace: espacio1 #Este es la línea que indica en que namespace debe crearse
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
~~~

Una vez aplicado este manifiesto con `kubectl apply -f pod.yaml`, ejecutamos una consulta de todos los objetos  en el namespace espacio1

`kubectl get all --namespace espacio1`

Una forma alternativa de crear un objeto en un namespace específico sería pasar le opción -n `<nombre_namespace>` en el comando kubectl apply. Con esta forma de crear el objeto, el archivo de manifiesto no necesita tener el campo namespace.

`kubectl apply -f pod.yaml -n espacio1`

## Eliminar in namespace

Para eliminar un namespace usamos el mismo comando que elimina objetos `kubectl delete ns espacio1` sin embargo hayq eu tener cuidado dado que elimina todos los elementos contenidos dentro del namespace.

## Cambiar el namespace por defecto

Se ha dicho que el namespace por defecto de k8s es **defaul namespace**, eso quiere decir que cuando creamos un objeto y no especificamos un ns creará en el default.

Esto podría resultar molestoso cuando queremos trabajar en otro namespace, para lo cual podemos cambiar el namespace por defecto con el siguiente comando:

`kubectl config set-context --current --namespace=espacio1`
espacio1 es el nombre del namespace creado.
