# Instalación de un cluster kubernetes

Vamos a usar k3s para trabajar en este taller

## Instalación de k3s

[Página de k3s](https://k3s.io/)

Ejecutar el script

`curl -sfL https://get.k3s.io | sh -`

y listo

`sudo k3s kubectl get nodes`


## Instalación de un nodo worker al cluster 

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


## Kubectl

Es la herramienta de línea de comandos para la comunicación con el control plane del cluster usando el API de kubernetes.

*kubectl [verbo] [objeto|recurso] [Nombre_recurso] [parámetros]*

- verbo: Especifica la acción que se quiere realizar: get|create|describe|delete
- objeto|recurso. Especifica el tipo de recurso que queremos del cual se desea la ejecutar la acción:
- Nombre recurso: Nombre del recurso
- parámetros. Dependiendo de la acción cada comando tendrá diferentes parámetros

## Configurar kubelet en el nodo worker para que trabaje con el cluster

Copiar el archivo del nodo master: 
**/etc/racher/k3s/k3s.yaml** a una ubicación del nodo worker. En este ejemplo lo copiamos a la ubicación:
**/root/.kube/config**

En el nodo worker editar el archivo k3s.yaml y poner la ip del nodo master. Despues crear la variable de entorno:

export KUBECONFIG=/root/.kube/config

Validar con el comando kubectl get nodes

## Error de certificado entre worker y master

Se podría tener un error en el certidicado para la comunicación entre el worker y el nodo master, si eso ocurre podremos:

Reiniciar el servicio master y el nodo worker para renovar el certificado
`sudo systemctl restart k3s`

En el worker node:

`reboot`
