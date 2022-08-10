# Storage o Almacenamiento

Al igual que en docker un volumen es un espacio de almacenamiento donde se guardan los datos de manera permanente y se puede acceder a ellos incluso despues que un contenedor se destruye.

Se conoce que las imágenes y contenedores son efímeros y que la información no es persistente. En docker usamos volumenes para dar la persistencia a los datos y asegurarnos que las aplicaciones puedan almacenar la información que generan y luego recuperarla.

En docker la solución era utilizar volumenes, y esto era montar un directorio del docker host en dentro de contenedor.

En kubernetes este sería un maniefiesto de pod con la declaración de un volumen:

~~~yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx2
  labels:
    aplicacion: miappmovil
    tipo: movil
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: data-volumen


  volumes:
    - name: data-volumen
      hostPath:
       path: /root/data/
       type: Directory

~~~

De este maniefiesto revisando un volumen podemos identificar dos partes.

~~~yaml
volumes:
  - name: data-volumen 
       path: /data 
       type: Directory 
~~~

Esta es la definición de un volumen. En donde:

*name*: es el campo para definir un nombre al volumen en nuestro caso data-volumen

*hostPath*: es el tipo de volumen que se va a crear. Se verá adelante que existe muchos tipos de volúmenes. hostPath es un tipo de volumen que monta un archivo o directorio del filesistem del nodo host al pod.

*path*: el path del nodo host que montaremos en el pod

*type*: se especifica si lo que se monetará es un directorio (Directory), archivo (File), Socket, DirectoryOrCreate, FileOrCreate

y la otra parte que se identifica es dentro de la definición del contenedor

***volumeMounts***, que es la sección del manifiesto donde monta el volumen dentro del pod.

~~~yaml
volumeMounts:
    - mountPath: /usr/share/html/
      name: data-volume
~~~

*volumeMounts*: El campo dentro de la especificación del contenedor para montar un volumen.

*mountPath* La ubicación dentro del contenedor donde se quiere montar el volumen.

*name* El nombre del volumen que queremos montar en el pod.

Esta sería un solución para un pod que se ejecuta en un solo nodo. Sin embargo, para kubernetes se añade una problema: ¿Como administrar la persistencia de los datos en un cluster donde varios nodos deben ver la misma información?

Y finalmente si pensamos que un cluster de kubernetes nos ofrece alta disponibilidad, el almacenamiento del cluster debe "sobrevivir" a una falla del cluster.

Entonces tenemos tres requerimientos para el almacenamiento (storage) en kubernetes:

1. El storage no debe depender del ciclo de vida del pod
2. El storage debe estar disponible para todos los nodos del cluster
3. El storage debe sobrevivir a una falla del cluster.

## Proyectos a storage para kubernetes

opensource
- longhorn
- openebs
- glusterfs

Enterprise
- portworx
- ondat