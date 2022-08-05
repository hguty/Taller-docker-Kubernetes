# Taint y Tolerance

Taint es un concepto dentro de kubernetes que permitirá a un nodo rechazar a ciertos pods y permitir a otros (a aquellso que tengan tolerancia).

El concepto de Tolerancia se aplica a los pods, un pod con cierta toleracia configurada permitirá al scheduler programarlo en los nodos para los cuales tiene tolerancia.

Taints y tolerancia son conceptos que trabajan juntos para asegurar que los pods no se ubiquen en los nodos inadecuados. Se aplican uno o varios  taints a los nodos y solo llegarán a estos aquellos pods que tengan cnfigurada su tolerancia.

## Aplicar taint a nodos

Primero debo aplicar los taints a los nodos a los cuales deseo "aislar".

`kubectl taint nodes nombre_nodo key1=value1:<taint-effect>`

nombre_nodo, Se debe especificar el nombre del noso al cual quiero aplicar el taint.

key1=value1 es la definición del taint

taint-effect. Es la acción que provoca a los pods que no tienen tolerancia a este taint. Hay tres posibles efectos que se pueden configurar:
  
    - NoSchedule Los pods no serán ubicados en este nodo
    - PreferNoSchedule El scheduler preferiblemente intentarán no ubicar los pods en este nodo pero no garantiza que esto se cumpla.
    - NoExecute los pods que no toleren el taint serán desalojados inmediatamente y los pods que sí la toleren nunca serán desalojados. Sin embargo, una tolerce con el efecto NoExecute puede especificar un campo de toleranciaSeconds opcional que dicta cuánto tiempo permanecerá vinculado el pod al nodo después de agregar la contaminación. 

## Configurar tolerance a un pod

Para configurar la tolerancia de un pod a un taint se debe especificar en campo **tolerations** en la sección spec del pod con los siguientes parámetros:

    ~~~yaml
    tolerations:
    - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
    ~~~

Alternativamente se puede usar el operador **Exists** en lugar de Equal, de la siguiente manera:

    ~~~yaml
    tolerations:
    - key: "key1"
    operator: "Exists"
    effect: "NoSchedule"
    ~~~

Ejemplo.

Crear dos taint en el nodo worker1

`kubectl taint nodes worker1 app=webapp:NoSchedule`

Ahora crearemos un pod con tolerancia al tain que acabamos de crear:

    ~~~yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx
    labels:
        env: test
    spec:
      containers:
       - name: nginx
        image: nginx
      tolerations:
       - key: "app"
         operator: "Equal"
         value: "webapp"
         effect: "NoSchedule"
    ~~~

## Notas importantes

Algunas notas importantes respecto a taints y tolerance:

- Un pod con tolerancia al taint de un nodo no garantiza que vaya a ese nodo en particular. Para eso hay otro concepto como node afynity.
- El nodo master tiene configurado un taint por defecto, que previene que los pods usen este nodo como un worker. Podemos ver esta configuración si hacemos un describe del nodo master: `kubectl describe node <masternodo> | grep taint`
- Se puede colocar multiples taints en el mismo nodo y multiples tolerations a un mismo pod. La forma en que kubernetes procesará esta configuración es como un filtro: empieza con todos los taints de un nodo y luego ignora aquellas para las que el pod tiene una tolerancia coincidente; los taints restantes no ignoradas tienen los efectos indicados en el pod.
- Si se agrega un taint con el efecto NoExecute a un nodo, los pods que no toleren el taint serán desalojados inmediatamente y los pods que sí la toleren nunca serán desalojados. Sin embargo, una tolerancia con el efecto NoExecute puede especificar un campo de **toleranciaSeconds**  que dicta cuánto tiempo permanecerá vinculado el pod al nodo después de agregar el taint. 
