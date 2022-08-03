# Node Affinity

El proposito principal de Node Affinity es asegurar la ubicación adecuada de un pod en un nodo, de acuerdo a reglas un poco mas avanzadas y complejas.

Para el uso de Node Affinity, al igual que el Nodo Selector debemos añadir labels a los nodos, el cambio se dará en los parámatros del archivo de manifiesto de los pods indicando a donde podrían ir.

Si quisieramos hacer la misma configuración que node selector para asignar a los nodos con label size=Large, el archivo de manifiesto sería similar al siguiente:

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
  affinnity:
    nodeAffinity:
      requiredDuringSchadulingIgnoredDuringExecution:
         nodeSelectorTerms:
         - matchExpressions:
            - key: size
              operator: In
              values:  
              - Large
~~~

Y aunque se ve un poco mas compleja que nodeSelector, con estos parámetros podremos tener un mayor alcance en la reglas que vamos creando. Por ejemplo, si quisíeramos indicar al pod que se ubique en cualquier nodo con label Large o Medium, nos vastaría con agregar el valor Medium en la lista de values de matchExpression se asume que hay nodos que tiene también el label size=Medium).

~~~yaml
  affinnity:
    nodeAffinity:
      requiredDuringSchadulingIgnoredDuringExecution:
         nodeSelectorTerms:
         - matchExpressions:
            - key: size
              operator: In
              values:  
              - Large
              - Medium
~~~

## Antiaffinity 

Podemos poner reglas también para evitar que se vaya a un nodo con reglas notIn. En el ejemplo anterior, en el caso de querer indicar que se vaya a cualquier nodo QUE NO SE "LARGE" el manifiesto sería:

~~~yaml
  affinnity:
    nodeAffinity:
      requiredDuringSchadulingIgnoredDuringExecution:
         nodeSelectorTerms:
         - matchExpressions:
            - key: size
              operator: NotIn
              values:  
              - Large
~~~

## Operator

Podemos encontrar estos tipos de operadores:

- In El operador In, asegura asegura que el pod se ubicará en un nodo cuyo label size tenga alguno de los valores listados en el campo values.
- NotIn Este operador se puede usar para decir en que nodo no debería ir el pod. 
- Exists Este operador solo valida la existencia de un label, no mira su valor, por lo que no se requiere el campo values:
- DoesNotExist. Ubicará un pod en los nodos donde no esté configurado el label indicado en el campo key. No se requiere conocer el valor de key.

## Node Afiinity Types

La sentencia mas larga de la configuración de nodeaffinity en el ejemplo es: **requiredDuringSchedulingIgnoredDuringExec**
 y guarda la información del comportamiento de pod durante en tiempo de planeación y en tiempo de ejecución.

Si nos fijamos en esta sentencia realmente es una oración en dos parte: La primera parte me indica que el comportamiento del pod en la fase de scheduling (DuringScheduling) es decir mientras busca el nodo, y la segunda parte indica el comportamiento del pod en la fase de ejecución (DuringExecution), es decir mientras el pod está corriendo puede ser que cambie su entorno como que un nodo elimine o añada un nuevo label.

|        | DuringScheduling | DuringExecution |
|--------|------------------|-----------------|
| Tipo 1 | Required         | Ignored         |
| Tipo 2 | Preferred        | Ignored         |

Tipo 1. Durante la fase scheduling buscará un nodo que cumpla la reglas de afinidad configuradas, si no encuentra un nodo que cumpla el pod no podrá ser ubicado y queda en estado *pending* hasta encontrarlo. Por otro el pod que se encuentre ejecutando en un nodo que cambia o elimina su labels ingorará estos cabios. 

Tipo 2. Si queremos que la regla de afinidad no sea tan estricta podemos usar preferredDurinScheduling, en este caso el pod buscará un nodo que cumpla la regla pero si no la encuentra podrá ubicarse en otro nodo disponible. En este tipo de afinidad los pods también ignoran los cambios en los pods en fase de ejecución.

Tipo 3. Según la documentación de kubernetes se planea un tercer tipo de node affinity. En este caso tanto en fase de scheduling como de ejecución el pod no podrá ubicarse en un nodo que no cumplas las reglas definidas.

