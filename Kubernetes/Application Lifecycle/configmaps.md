# ConfigMaps

Un configmap es un objeto de k8s para guardar datos no confidenciales en forma de clave-valor.
Valor de configuracion que pasamos a los contenedores en el momento de su creación.

Los usos mas frecuente para usar configmap es para setear datos de configuración separados del código de la aplicación.

Un configmap puede almacenar hasta 1 MB de información. Si se desea guardar mas información se debe usar volumenes o una base de datos de configuraciones.  

## Crear un configmaps

`kubectl create configmap prueba-cm --from-literal=TITULO=SALUDOS --from-literal=MENSAJE=HOLAMUNDO`

También se puede crear configmaps a partir de un archivo de manifiesto que tendría una estructura similar a esta:

~~~yaml
apiVersion: v1
kind: ConfigMap
metadata:
   name: prueba-cm
data:
  TITULO: SALUDOS
  MENSAJE: HOLA MUNDO
~~~

Un tercera alternativa es crear un configmap es especificar el path de un archivo que contiene los datos de clave-valor

`kubectl create configmap prueba-cm --from-file=app_config.properties`

El archivo deberá contener los datos de clave valor que necesitamos pasar a la áplicación:

Ver los configmaps creados:

`kubectl get cm`

`kubectl describe cm prueba-cm`

## Inyectar un configmap a un pod

Una vez creado un configmap este debe ser inyectado al pod para poder hacer uso de los datos creados.
Dentro de la especificación del pod se debe incluir las siguientes opaciones:

~~~yaml
envFrom:
  - configMapRef:
        name: prueba-cm
~~~

## Inyectar una única variable desde configmap a un pod

Se puede inyectar solo un variable específica de todo el configmap usando las siguiente instrucciones:

~~~yaml
env:
  - name: TITULO
    valueFROM:
       configMapKeyRef:
         name: prueba-cm
         key: TITULO
~~~

## Eliminar un configmap

`kubectl delete configmap prueba-cm`

:
