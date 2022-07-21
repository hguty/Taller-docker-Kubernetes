# Controlers

Los controler son el cerebro detras de kubernetes. Pues ayudan a tomar desición cuando un componente falla y buscará los medios para asegura su funcionamiento.

## Replication controler

**Los replication controlers** ayudan a tener multiples instancias de pods ejecuntadose y no perder el acceso a la aplicación. esto provee alta disponibilidad.
Incluso si quieramos tener un solo pod ejecutandose necesitamos un replication controler por que es este quien asegurará que si falla el pod, otro ocupe su lugar. Entonces usaremos replication controler para tener 1 o 100 pods en ejecución.

Los replication controlers también nos ayudarán a ir creando varias instancias de un pod cuando se incremente la carga a la aplicación es decir son necesarios para hacer un balanceo de carga y escalado de cargas de trabajo. Si importar con cuentos nodos trabaje mi cluster el replication controler irá asignando un pod en algún nod disponible del cluster, como se puede ver en la siguiente figura.

![load balancing and scaling](../img/loadb.jpg)

### Creación de replication controler

La creación de un replication controler sigue el mismo patrón del manifest de todo objeto en kubernetes:

![Replication controler secciones](../img/rc-seccion.jpg)

La sección de spec tendrá los parámetros necesario para este tipo de objetos. 

El parámetro requerido es template para indicar que tipo de pod se creará con el replication controller. Dentro del campo template irá la misma especificación como si se creará un pod. Se muestra en la siguiente figura:

![Replication controler secciones](../img/rc-controler-1.gif)

Se completa la sección spec del replication controler con la propiedad *replicas* que indica  cuantos pods se ejecutan en paralelo

Un manifisto completo de RC con 3 réplicas de pod que contiene el contenedor de nginx quedaría así:

~~~yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp
  labels:
     app: myapp
     tipo: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: <Image>
          ports:
            - containerPort: <Port>
~~~

Note la estructura del manifiesto:
Dos secciones de *metadata* (una para los datos del Replication controller y otra para los datos del pod).
Dos secciones de spec (Una para las propiedades de RC y otra para loas propiedades del pod).

La ejecución del RC sería similar a lo visto aneriormente, esto es:

`kubectl apply -f rc-definition.yaml`
