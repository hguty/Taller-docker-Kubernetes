# Scheduling en kubernetes

Recordar que el componente de scheduler dentro de la arquitectura de kubernetes, sirve para ubicar un pod en los nodos disponibles en el cluster. 

Cuando un nuevo pod que se está creando el kube-scheduler va a buscar el nodo mas óptimo.
Esta desición puedes parece fácil, pero se debe pensar en que cada contenedor dentro del pod tendrá un conjunto requerimiento de recursos, y cada pod tendrá otro requerimiento diferente de recursos. Entonces el proceso de schedulling debe filtrar únicamente los nodos que cumplan con esos requerimientos.

## proceso de selección de un nodo.

Los nodos del cluster que cumplen con los requsitos para albergar un pod se llama *feasible nodes*. Cuando un pod no encuentra nodos adecuados, el scheduler lo dejará como un pod  unschedule (sin asignación) hasta que se lo pueda colocar.


Para realizar esta tarea existen varios conceptos que vamos a revisar como:

- Manual schedulling
- Labels & Selectors
- Taints & Tolerations
- Node Selector, Node Affinity
- Resources Limits
- DaemonSets
- Static Pod