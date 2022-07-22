# Deployment

En un objeto de tipo deployment es una descripción declarativa de un estado deseado para pods y replicaset.

Un deployment es un objeto que administra pods y replicaset, y añade ciertas funcionalidades necesarias para despliegue de aplicaciones como;

- Despliegue automático de contenedores en la cantidad deseada. (Réplicas de pods)
- Facilita el despligue de nuevas versiones de la aplicación a través de nuevas versiones de imágenes
- Despliegue controlado de las nuevas versiones de la aplicación. Rolling update
- Permite hacer un rollback de un despliegue si la aplicación tuviera un error no esperado (rolling back). Deshace el cambio reciente.  
- Permite pausar y reacer los cambios en el cluster según sea necesario

![Deployment](../img/arq_deploy.jpg)

De la figura se puede observar como los contenedores están encapsulados en los pods, los replicaset contienen réplicas de multiples pods, mientras que en los mas alto de esta jerarquía está los deployment que son los objetos que permiten administrar lo replicaset agregando mas funcionalidades.

## Manifiesto del deployment

El archivo de manifiesto o de definición de deployment es similar al de replicaset, con solo un cambio: el campo kind se especifica por **Deployment**

![Deploy definition file](../img/deploy_definition_file.jpg)

~~~yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deploy
  labels:
     app: movilapp
     tipo: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: movilapp
    spec:
      containers:
        - name: myapp-container
          image: nginx:1.22.0
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      app: movilapp
~~~

## Revisando los deployment's creados

Al igual que todos los objetos en k8s, podemos consultar con el siguiente comando:

`kubectl get deployment`

Cuando inspeccionas los Deployments del clúster, se podrán ver los siguientes campos:

- NAME muestra los nombre de los Deployments del clúster.
- READY muestra cuántas réplicas de la aplicación están disponibles.
- UP-TO-DATE muestra el número de réplicas que se ha actualizado para alcanzar el estado deseado
- AVAILABLE muestra cuántas réplicas de la aplicación están disponibles para los usuarios
- AGE muestra la cantidad de tiempo que la aplicación lleva ejecutándose.

En el mismo comando si agregamos la bandera `-o wife` podremos visualizar mas campos del deploy




