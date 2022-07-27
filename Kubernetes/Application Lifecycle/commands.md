# Command y argumentos a un pod

Al igual que el docker se pueden pasar comandos y argumentos en la instanciación de una imagen, en kubernertes tambien podemos definir comandos y argumentos para la ejecución de comandos.

En el archivos de maniefiesto del pod usamos los campos: `command` que sería el equivalente de *entrypoint* en docker y `args` que sería para los argumentos del coammando.

Los comandos y los argumentos que se definen no pueden ser cambiados una vez haya sido creado el pod.
Los comandos y argumentos que se definan archivo de configuración sobreescriben los comandos y argumentos por defecto provisto por la imagen del contenedor.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: command-demo-container
    image: debian
    command: ["printenv"]
    args: ["HOSTNAME", "KUBERNETES_PORT"]
  restartPolicy: OnFailure
~~~

Aplicamos el manifiesto

`kubectl apply -f podcommand.yaml`

y verificamos la salida del pod.

`kubectl logs command-demo`

## Variables de entorno en un pod

Se puede definir variables de entorno dentro del archivo de manifiesto, usando el campo **env** en la sección **spec** del pod.


~~~yaml
env:
  - name: TITULO
    value: SALUDOS 
  - name: MESSAGE
    value: "hello world"

~~~

env: es el campo dentro de spec para crear variables de entorno en el contenedor.
name: el nombre de la variable de entorno
value: el valor para la variable de entorno

Se pueden definir varias variables de entorno separadas con un guion por cada nueva variable.

Las variables de entorno pueden pasar como argumentos de un comando a un contenedor:

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: command-demo-container
    image: debian
    env:
      - name: TITULO
        value: "Saludos"
      - name: MENSAJE
        value: "Hola mundo"
    command: ["/bin/echo"]
    args: ["$(TITULO)", "$(MENSAJE)"]
  restartPolicy: OnFailure
~~~

## Ejecutar un comando en un contenedor en ejecucióm

`kubectl exec <pod_name> <comando>`

`kubectl exec myapp-pod env`

## Ingresar al bash de un contenedor

`kubectl exec -i -t <pod_name> -- /bin/bash`

## Ingresar al bash de un pod con varios contenedores contenedor

`kubectl exec -it <my-pod_name> --container <container_name> -- /bin/bash`
