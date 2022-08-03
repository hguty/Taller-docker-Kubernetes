# Scheduling en kubernetes

Recordar que el componente de scheduler dentro de la arquitectura de kubernetes, sirve para ubicar un pod en los nodos disponibles en el cluster.

Cuando un nuevo pod que se está creando el kube-scheduler va a buscar el nodo mas óptimo.
Esta desición puedes parece fácil, pero se debe pensar en que cada contenedor dentro del pod tendrá un conjunto requerimiento de recursos, y cada pod tendrá otro requerimiento diferente de recursos. Entonces el proceso de schedulling debe filtrar únicamente los nodos que cumplan con esos requerimientos.

## Proceso de selección de un nodo

Los nodos del cluster que cumplen con los requsitos para albergar un pod se llama *feasible nodes*. Cuando un pod no encuentra nodos adecuados, el scheduler lo dejará como un pod  unschedule (sin asignación) hasta que se lo pueda colocar.

## Agregar un nodo worker al cluster k3s

Hasta el momento se ha trabajando con un cluster de un solo nodo (master node), un cluster de producción de kubernetes va a tener varios nodos workers. Vamos a realizar la instalación y pegarle al cluster de k3s que tenemos en funcionamiento.

1. Encontrar el token del nodo master

    `cat /var/lib/rancher/k3s/server/node-token`

2. En el nodo worker que queremos atachar al cluster vamos a crear 2 variables de entorno:
   - K3S_URL que es la dirección ip y puerto del nodo master `k3s_url="https://192.168.1.94:6443"`
  
   - K3S_TOKEN que es el token del nodo master `k3s_token="K101b8f68517d06ed0786b49f3afaf8cefc6c35e6c0d11ec716d778eb318c98b330::server:c5324d25197dde5981b7732ee3a30779"`

3. Descargar y ejecutar el script de k3s con los parámetros del paso anterior

    `curl -sfL https://get.k3s.io | K3S_URL=${k3s_url} K3S_TOKEN=${k3s_token} sh -`

4. De regreso al nodo master ejecutamos una consulta de los nodos del cluster
   `kubectl get nodes`

## Ejecutar un deploy

Ahora que tenemos un cluster con otro nodo worker vamos hacer instanciar un deploy con 3 réplicas y validemos cual es la distribución que le ha asignado el scheduler de nuestro cluster

`kubectl apply -f deploy.yml`

`kubectl get pods -o wide`

### Contenidos de sheduller

Para cubrir el contendo de scheduler vamos a revisar varios conceptos:

- Manual schedulling
- Labels & Selectors
- Taints & Tolerations
- Node Selector, Node Affinity
- Resources Limits
- DaemonSets
- Static Pod
  