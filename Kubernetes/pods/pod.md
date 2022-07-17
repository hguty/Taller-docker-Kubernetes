# POD

Un POD representa la ejecución de uno o varios contenedores en el cluster.
No vamos a manipular un contenedor sino un pod.

Aunque un pod puede contener varios contenedores, lo más utilizadó será una relación 1 a 1 es decir un contenedor por pod.

La creación de un pod desde linea de comandos sería:

``

La ejecución del mismo pod utilizando un manifest. Crear un archivo simplepod.yaml con el siguientes texto

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
spec:
  containers:
  - name: webserver
    image: nginx:latest
    ports:
    - containerPort: 80
~~~

Analizando el contendio del archivo
![Seccion yaml](img/seccion_yaml_file.jpg)

Está sección de la imagen muestra los campos comunes que tendrán todos los objetos en kubernetes.  
ApiVersion. es la instrucción que especifica la versión del api de kubernetes usado para crear este objeto
kind. Es el tipo de objeto (pod, deploy, replicaset, daemonset, service. etc)
metadata: Datos que permiten identificar al objeto, como su nombre, namespace, ambiente, autor, ect

![spec de pod](img/spec_pod.jpg)

La siguientes sección es spec, en donde se indíca las especificaciones técnicas del objeto. Cada objeto tendrá sus propios parámetros e instrucciones. Siempre se revisa la docuemntación oficial de los objetos en kubernetes.

Para el caso de un pod los campos o atributos que llenamos son:

- containers: Es una instrucción de tipo list/array eso quiere decir que puedo tener varios contenedores en un mismo pod.
- name: Identado dentro de la lista containers y empezando con un \- indicamos el nombre del primer contenedor que vamos a crear, en nuestro ejemplo **webserver**. Cuando en un pod solo se crea un contenedor es frecuente llamar al contenedor con el mismo nombre del pod. Si se quiere agregar mas contenedores en un mismo pod, se iniciará con otro name identado con un \- en una nueva linea a la misma altura que el primer contenedor.
- image. La imagen del contenedor. Si usas otros repositorio asegurase de especficar la ruta completa de la imagen.
- ports: es una lista o array de los puertos donde va a escuchar el contenedor  
  
## Desplegando un pod

Para desplegar el pod con su archivo yml se requiere del siguiente comando:
`kubectl apply -f mipod.yaml`

## Comprobar estado de los pods

Una vez que se ejecuta el comando de ejecución del pod podemos listar los pods para ver su estado de ejecución con el comando:

`kubectl get pods`

Este comando no ofrecerá los siguientes la visualización de los siguientes campos:

NAME: Nombre del pod, el cual se especificó en el campo name de metadata.

READY: El número de contenedore por pod en ejecución.

STATUS: El estado del pod \<ciclo de vida del pod\> 
Los posibles estados son:

- pending. El pod a sido aceptado en el cluster pero uno o mas contenedores aun no están en ejecución. Este estado incluye el tiempo mientras el pod espera el scheduling o mientras se descarga la imagen de un contenedor
- running. El pod a sido ubicado en un nodo y sus contenedores han sido creados.
- succeded. Todos los contenedores del pod finalizaron correctamente y no se reiniciarán.
- failed. Todos los contenedores finalizaron pero al menos uno terminó en fallo.
- unknown. Por alguna razón no se puede obtener el estado del pod. Este fallo generalmente se debe por una falla con la comunicación del nodo donde se está ejecutando el pod.

RESTARTS: El número de reinicios del pod

AGE: El tiempo de vida desde la útlima ejecucióm

Un comando que me permite ver campos adicionales del pod sería:

`kubectl get pods -o wide`

## labels

Dentro de la sección metadata se agrega etiquetas me permite identificar y filtrar pods o elementos del cluster. En el siguiente ejemplo agrego 3 etiquetas app=appmovil, tipo= movil y ambiente=test
Las estiquetas de tipo clave valor.

Vamos a agregar la sección labels al archivo de pod original de la siguiente manera:

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
  labels:
     app: appmovil1
     tipo: movil
     ambiente: test
spec:
  containers:
  - name: webserver
    image: nginx:latest
    ports:
    - containerPort: 80
~~~

Para ejecutar estos cambios no hace falta eliminar el pod anterior, simplemente ejcutamos el comando  `kubectl apply -f https://k8s.io/examples/pods/simple-pod.yaml` y veremos que el pod actualiza la información.

## Ver el detalle de un objeto en kubernetes

Cada objeto tiene información propia de su naturaleza así como metadata. Para ver la información usamos el comando: `kubectl describe <tipo_objeto> nombre objeto` para este el caso del pod que hemos creado sería:
`kubectl describe pod mynginx`

