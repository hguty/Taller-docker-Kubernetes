# POD

Un POD representa la ejecución de uno o varios contenedores en el cluster.
No vamos a manipular un contenedor sino un pod.

Aunque un pod puede contener varios contenedores, lo más utilizadó será una relación 1 a 1 es decir un contenedor por pod.

La creación de un pod desde linea de comandos sería:

` `

La ejecución del mismo pod utilizando un manifest 

~~~
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: webserver1
    image: nginx:latest
    ports:
    - containerPort: 80
~~~
