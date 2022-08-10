# Resources

El proceso de scheduler valida la cantidad de recursos (cpu y memoria) que requiere el pod y los recursos disponibles en cada nodo. Cuando encuentra un nodo que satisface los requerimientos lo ubica. Cuando no encuentra un nodo para para un pod lo dejará en estado pendiente (pending) hasta que se liberen los recursos necesarios.

Se puede configurar la cantidad de memoria ram y procesador que requiere el contenedor dentro del pod y con esta información el scheduler buscará el nodo mas óptimo. 

![Resources](../img/resources.png)

## CPU Units

La unidad de medida del CPU en kubernetes es CPU Unit y equivale a 1 core físico (para servidores físicos) o 1 virtual core (para un servidor virtual). Con esta unidad de medida se pueden configurar fracciones, por ejemplo 0.1 equivaldrá a 100 m o se lee *cien miliCPU*. En ocasiones es preferible usar la expresión 0.8 m para referirnos a la cantidad de recursos 

El recurso de CPU siempre se especifica como una cantidad absoluta de recursos, nunca como una cantidad relativa. Por ejemplo, 500 m de CPU representan aproximadamente la misma cantidad de potencia informática, ya sea que ese contenedor se ejecute en una máquina de un solo núcleo, de dos núcleos o de 48 núcleos.

Kubernetes no le permite especificar recursos de CPU con una precisión inferior a 1 m. Debido a esto, es útil especificar unidades de CPU como 1.0 o 1000m utilizando este formato milliCPU; por ejemplo, 5 m en lugar de 0,005.

## Memoria Units

La unidad de medida para la memoria son los bytes. Se puede trabajar tambien con los subfijos: T, G, M, k para referirse al subfijo Terabyte, Gigabyte, Megabyte o Kilobyte.

Se puede especificar tambien los subfijos Gi, Mi, Ki para referirse a las unidades Gigabit, Megabit y Kilobit.

## Resource request

Un requerimiento de recursos (request) indicará cuanto es el mínimo de cpu y memoria que necesita un pod para poder inicializar. Kubernetes debe hacer una reserva de esta cantidad de resursos por lo cual no podrá asignar un nodo que con satisfaga los valores de request.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
    resources:  #Parámetro que indica las sección de recursos
       requests:  #Dentro de los recursos es el parámetro para indicar los recursos    requeridos
         memory: "1G"
         cpu: 0.8 
~~~

Este manifiesto asegura para el nuevo pod 1 GB de memoria y 800 miliCPU; el scheduler buscará un pod que tenga estos recursos mínimos requeridos.

## Resources limits

Kubernetes no establece un límite por defecto para los recursos, sin embargo es una buena práctica configurar estos parámtros para cada pod y asegurarnos el control de su ejecución.

Para especificar los valores debemos indicar al manifiesto es la sección de resources unos límites para CPU y Memoria, usando las unidades indicadas previamente.

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
  labels:
    aplicacion: miappmovil
    tipo: movil
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 8040
    resources:
      requests:
        memory: "600M"
        cpu: 0.5
      limits:
        memory: "800M"
        cpu: 0.8 
~~~

Si el pod supera el límite establecido el pod puede detenerse, si el pod supera el CPU por periodos cortos de tiempo no se detendrá.
