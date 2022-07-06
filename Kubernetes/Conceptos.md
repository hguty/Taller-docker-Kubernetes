# Arquitectura y componentes de kubernetes

Cuando desplegamos kubernetes tenemos un cluster. que son un conjunto de máquinas que ejecutan las cargas de trabajo (pods), estas máquinas son llamados **nodos (worker nodes)**. Existe el nodo master que es el encargado de administrar el cluster. Este nodo master  se conoce como **control plane** y cuenta con algunos componentes para llevar el control del cluster 


![Arquitectura kubernetes](img/arqkub.jpg)

## Componentes del nodo (worker node)
- kubelet Es un agente que se ejecuta en cada nodo y asegura que los pods se ejecuten.  
- kube-proxy es un proxy de red que se ejecuta en cada nodo y en cargado de mantener las reglas de networking para la comunicación de los pods con el cluster y los servicios de kuberntes
- Container runtime Este componentede software es el responsable de la ejecución de los contenedores
  

## Componentes del control plane
- kube-apiserver. Un servidor API es un componente del control plane que expone la API de Kubernetes. Es el front-end del control plane de Kubernetes.

La implementación principal de un servidor API de Kubernetes es kube-apiserver. kube-apiserver está diseñado para escalar horizontalmente, es decir, escala mediante la implementación de más instancias.
- etcd. Es el almacen de datos clave valor, que guarda información del cluster. Kubernetes almacena todos los objetos del RESTful API en este componente. 
- Scheduler (kube-scheduler) Evalua la capacidad y los recursos de cada nodo para decidir a cual de ellos enviar las cargas de trabajo (pods)   
- Controller-manager (kube-controller-manager) Este componente ejecuta proceso de control del cluster como: validar la disponibilidad de los nodos, control de trabajos, control de los accesos por cada namespace 
 