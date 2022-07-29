# Liveness and Readiness Probes

## Liveness

Livenes sirve par que kubernetes sepa si un pod esta "vivo", Kubelet usa liveness para conocer si debe reiniciar o no un pod. Hace un prueba definida por el usuario, si la prueba es satisfactoria concidera que el pod esta vivo y todo sigue trabajando. Cuando la prueba falla considera que el pod tiene alguna falla y lo reinicia automáticamente 

Hay como hacer tres tipos de pruebas usando liveness

### Liveness command

Es la ejecución de un comando para chequear el estado de vida de un contenedor. 

Ejemplo. El siguiente pod validamos la existencia del archivo index.html, mientras el archivoo existe la prueba es satisfactoria y el pod trabaja sin problema.  

~~~yaml
# YAML example
# liveness-pod-example.yaml
#
apiVersion: v1 
kind: Pod 
metadata: 
  name: liveness-command-exec 
spec: 
  containers: 
  - name: liveness 
    image: nginx 
    ports: 
        - containerPort: 80 
    livenessProbe: 
      exec:
        command:
        - cat
        - /usr/share/nginx/html/index.html
      initialDelaySeconds: 2 #Default 0 
      periodSeconds: 2 #Default 10 
      timeoutSeconds: 1 #Default 1 
      successThreshold: 1 #Default 1 
      failureThreshold: 3 #Default 3
~~~

Algunos parámetros por revisar:

- initialDelaySeconds – Es el tiempo que tarda despues de que arranca un contenedor para empezar hacer el liveness probe.
- periodSeconds Es la frecuencia con la que hace la prueba.
- timeoutSeconds – tiempo después del cual expirará el tiempo de espera.
- successThreshold – el número mínimo de intentos exitosos después de los cuales la sonda determinará el funcionamiento correcto del contenedor.
- failureThreshold – número de intentos fallidos después de los cuales el contenedor se reiniciará 

Para validar el funcionamiento de la prueba, vamos a eliminar el archivo: /usr/share/nginx/html/index.html Si esta prueba falla, es decir no encuentra el archivo kubernetes considera un fallo y reinicia automáticamente el pod.

### Liveness HTTP request

Esta prueba de liveness busca una respuesta de http si recibe un código de error reiniciará el pod.

Algunas de las opciones de liveness httpGet son:

host – El hostname a donde conectar. por default se conecta a la IP del POD. 
scheme – Especifica el tipo de protocolo  HTTP or HTTPS
path – Path de conexión
httpHeaders– Se podría customizar el headers para el request http 
port – Se podría personalizar el puerto de acceso al contenedor

~~~yaml
apiVersion: v1 
kind: Pod 
metadata: 
  name: liveness-request 
spec: 
  containers: 
  - name: liveness 
    image: nginx 
    ports: 
        - containerPort: 80 
    livenessProbe: 
      httpGet: 
        path: / 
        port: 80 
      initialDelaySeconds: 2 #Default 0 
      periodSeconds: 2 #Default 10 
      timeoutSeconds: 1 #Default 1 
      successThreshold: 1 #Default 1 
      failureThreshold: 3 #Default 3 
~~~

Para probar el pod vamos a borrar el index de nginx

`kubectl exec -it liveness-request -- rm /usr/share/nginx/html/index.html`

### Liveness Liveness TCP probe

Un tercer tipo de sonda de actividad utiliza un prueba de conexión TCP. Con esta configuración, el kubelet intentará abrir un socket a su contenedor en el puerto especificado. Si puede establecer una conexión, el contenedor se considera saludable, si no puede, se considera una falla.

~~~yaml
livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
~~~

## Readiness Probe.

A veces, las aplicaciones no pueden atender el tráfico temporalmente. Por ejemplo, una aplicación puede necesitar cargar archivos de configuración o datos grandes durante el inicio, o depender de servicios externos después del inicio. En tales casos, no desea eliminar la aplicación, pero tampoco desea enviarle solicitudes. Kubernetes proporciona readiness probe para detectar y mitigar estas situaciones. Un pod con contenedores que informan que no están listos no recibe tráfico a través de Kubernetes Services.

Readliness se configuran de manera similar a las liveness. La única diferencia es que usa el campo readinessProbe en lugar del campo livenessProbe.

Ejemplo.

Vamos a correr 2 pods, el primero con un prueba exitosa de readiness y liveness y un segundo pod con una pruba fallida de readiness para ver el comportamiento en cada caso.

Pod name: nginx

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 80
      initialDelaySeconds: 15
      periodSeconds: 20

~~~

Pod name: nginx-fail

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-fail
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    readinessProbe:
      httpGet:
        path: /hola
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 80
      initialDelaySeconds: 15
      periodSeconds: 20
~~~

