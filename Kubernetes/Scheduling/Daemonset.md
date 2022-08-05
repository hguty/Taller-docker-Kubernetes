# DaemonSets

DaemonSet es un objeto de kubernetes que nospermite tener varias replicas de un pod corriendo em el cluster, similar a un RS o deploy, con la particularidad que garantiza de que un réplica del pod se ejecute en cada nodo del cluster.

Este tipo de objeto será ideal para soluciones monitoreo o recopilación de logs en cada nodo del cluster.

DaemonSet permitirá que cada nodo que se una al cluster implemente o despliegue una copia del pod en ese nodo. De igual forma cuando un nodo se elimine del cluster, el pod se eliminará. 

## Casos de uso.

Los casos de uso mas frecuente que podemos tener para daemonset son:

- Para el monitoreo de los nodos se requiere un demonio que provea información del nodo. Un herramienta muy conocida es **Prometheus**
- Un demonio que haga rotación y limpieza de archivos de logs.
- Para ejecutar un demonio de cluster storage en cada nodo. Ejemplo: glusterd y ceph.
- Para corre un demonio de recolección de logs del nodo. Por ejemplo logstach o fluentd.

## Manifiesto de DaemonSet

El archivo de manifiesto de daemonset es similar a un archivo de replicaset, con dos cambios: el parámetro kind se especifica como DaemonSet y el otro cambio respecto a un manifiesto de rs es que no consta el parámetro réplicas. Este es evidente por que la cantidad de réplicas ira de la mano con los nodos en ejecución. 

~~~yaml
apiVersion: apps/v1 #La misma versión usada para RS
kind: DaemonSet #El único cambio respecto al manifiesto de RS
metadata:
  name: myapp-rs
  labels:
     app: movil-app
     tipo: front-end
spec:
  template:
    metadata:
      name: myapp
      labels:
        app: movilapp
    spec:
      containers:
        - name: myapp-container
          image: nginx
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      app: movilapp
~~~

Suele ser una práctica habitual restringir usar daemonset junto con taint y tolerance para agrupar solo ciertos nods en los cuales se decea desplegar el daemonset.

