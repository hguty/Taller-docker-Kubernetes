# DNS para servicios y pods

Kubernetes crea registros DNS para servicios y pods. Se puede contactar a un servicio por su nombre o su dirección IP. Este servicio de DNS es interno del cluster y no hay que confundir con el servicio DNS interno de la organización.

Kubernetes DNS programa un DNS Pod y Service en el cluster, y configura los kubelets para decir a cada contenedor la IP del DNS Service's para resolver los nombres.

`kubectl exec <pod> -- cat /etc/resolv`

Cada servicio definido en el cluster (incluyendo el mismo server DNS) se le asigna un DNS name. Por defecto, un cliente dns se configura en cada POD para resolver y buscar  dentro del propio namespace con el dominio por defecto del cluster.

Para ver el servicio dns dentro de un cluster podemos hacerlo mediante el comando:

`kubectl describe service kube-dns -n kube-system | more`

Buscar la ip del servicio

`kubectl get pods -n kube-system`
`kubectl describe pod <podname -n kube-system`

`kubectl get rs -n kube-system`

`kubectl get deploy -n kube-system`

`kubectl get service -n kube-system`

Responder:

- Cuantos pods tiene el servicio dns 
- Cuál es el nombre del servicio para acceder a CoreDns
- Cuál es la IP del servicio CoreDNS
- Donde se encuentra el archivo de configuración CoredDNS que se ejecuta en los pods.
  
## Namespaces de Servicios

Una consulta DNS puede retornar diferentes resultados basado en el namespace del pod.
Una consulta DNS que no especifica el namespace se limitará al namespace del pod
Para acceder a servicios de otros namespaces se lo podrá hacer especificando el nombre del namespace.

<servicio.namespace.svc.cluster.local>

## Namespace de Pod dentro de ns

La forma en que dns de kubernetes identifica al pod es:

<ip-addres.nameserver.pod.cluster.local>

## Hostname y subdominio

Cuando un pod es creado su hostname viene del campo: metadata-name. Sin embargo se podría personalizar el hostname y el sundominio de un pod. Los parámetros dentro de la definición del pod son: **hostname** y **subdomain**

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
  hostname: hgnginx
  subdomain: coop-chibuleo
~~~

Ver el contenido de /etc/hosts dentro del contenedor

`cat /etc/hosts`

## Añadir entradas a /etc/hosts

Se puede añadir entradas al archivo /etc/hosts en tiempo de creación y reinicio. Esto podría ser una solución si un dns no resuelve algún host y se requiere una resolución manual.

Aunque se puede modificar este archivo en tiempo de ejecución, sería temporal debido a la naturalesa de lectura de las imágenes de contenedores.

Para añadir entradas a /etc/hosts usamos el campo hostAliases dentro de la espec del pod.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
spec:
  hostAliases:
  - ip: "127.0.0.1"
    hostnames:
    - "serv01.local"
    - "server01.local"
  - ip: "10.1.2.3"
    hostnames:
    - "serv01.remote"
    - "server01.remote"
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
  
~~~

