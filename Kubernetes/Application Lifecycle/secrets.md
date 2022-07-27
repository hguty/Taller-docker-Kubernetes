# Secrets

Son objetos de kubernetes para guardar información sensible como passwords, tokens, o claves.

Son similares a un ConfigMap con la diferencia que por ser data sensible esta estará condificada.

Se requiere la creación del objeto secret con los parámetros de clave- valor y posteriormente inyectar los secrets dentro del pod. (Similar a los configmaps)

## Creación de secrets

Se lo puede realizar desde la linea de comandos de la siguiente manera:

`kubectl create secret generic <nombre_secret> --from-literal=DB_Username=admin --from-literal=DB_Password=mypass`

o desde un archivo de manifiesto yaml de declarando como tipo Seccret

~~~yaml
apiVersion: v1
kind: Secret
metadata:
   name: prueba-secreto
data:
  DB_UserName: YWRtaW4= 
  DB_Password: bXlwYXNz
~~~

Es un archivo de manifiesto muy parecido a configmap, con dos diferencia:

kind: será Secret, indicando que corresponde a este tipo de objetos.

En este arhivo de manifiesto los valores para DB_Username y DB_Password están codificados en base 64.

## Inyectar secret en un pod

Al igual que con configmap se debe especificar en la sección spec de la definición de pods con el campo:

~~~yaml
`envFrom`:
   - secretRef:
       name: prueba-secreto
~~~

Y la inyección de cierta variable dentro del secret sería:

~~~yaml
env:
   - name: DB_Password
     valueFrom:
        secretKeyRef:
           name: prueba-secreto
           key: DB_password
~~~

