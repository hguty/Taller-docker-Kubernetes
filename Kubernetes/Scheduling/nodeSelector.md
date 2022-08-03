# Node Labels

Un nodo puede tener también etiquetas al igual que otros objetos dentro de kubernetes. Para asignar label a un nodo usamos el comando:

`kubectl label nodes <nombre_nodo> <key>=<value>`

`kubectl label nodes worker1 size=Large`

Para ver los label asignados a los nodos usamos el comando:

`kubectl get nodes --show-labels`

## nodeSelector

Una forma de asignar un pod a un nodo es mediante el parámetro nodeSelector en la spec del pod, en este parámetro indicaremos el label del nodo al cual se quiere asignar.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx 
  nodeSelector:
    size: Large
~~~

## Limitaciones de nodeSelector

NodeSelector es un solución cuando queremos asignar específicamente un pod a un nodo posibles nodos que cumplan un condición  en particular, pero existen algunas limitaciones de usar esta estrategia para ubicar los pods en un nodo. Por ejemplo, que ocurre si tenemos un requerimiento un tanto mas complejo. Por ejemplo, se quiere ubicar en un nodo con size: Largo o Medium; o si "NO QUEREMOS" ubicarlo en un nodo Large.
