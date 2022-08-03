# Manual Schedulling

**nodeName** es el campo dentro del manifiesto para identificar el nodo al cual quiero enviar un pod. Este campo por defecto no se lo pone en el maniefiesto y dejamos que el scheduler de kubernetes busque y seleccione entre todos los nodos cual es la mejor opció entre ellos.
Una vez que kubernetes los ha seleccionado este completa en campo **nodeName** dentro del pod y ubica nuestro pod

Podríamos asignar un pod a un node de manera manual solo el momento de la creación.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx-node
spec:
 nodeName: worker1 ##En este parámetro puedo especificar el nodo donde levantar el pod de forma manual
 containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 8040
~~~
