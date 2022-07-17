# Instalación de un cluster kubernetes

Vamos a usar k3s para trabajar en este taller

## Instalación de k3s

[Página de k3s](https://k3s.io/)

Ejecutar el script

`curl -sfL https://get.k3s.io | sh -`

y listo

`sudo k3s kubectl get nodes`

Si queremos agregar un nodo worker al cluster, necesitamos el token del nodo master que lo encontrasmos en este path:

*/var/lib/rancher/k3s/server/node-token*

En el nodo worker ejecutamos el comando

`sudo k3s agent --server https://ipserver:6443 --token ${NODE_TOKEN}`

Nota importante.
Sería util crear un alias al comando k3s kubectl para usar el comando único kubectl
`alias kubectl="k3s kubectl"`

## Kubectl

Es la herramienta de línea de comandos para la comunicación con el control plane del cluster usando el API de kubernetes.

*kubectl [verbo] [objeto|recurso] [Nombre_recurso] [parámetros]*

- verbo: Especifica la acción que se quiere realizar: get|create|describe|delete
- objeto|recurso. Especifica el tipo de recurso que queremos del cual se desea la ejecutar la acción:
- Nombre recurso: Nombre del recurso
- parámetros. Dependiendo de la acción cada comando tendrá diferentes parámetros
