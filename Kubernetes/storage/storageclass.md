# Storage Class

Con Persistent volume se requiere ir creando de manera manual los volúmenes en el proveedor y los objetos PV.

Con *storage class* se puede provicionar de manera automática los volumenes requeridos usando un proveedor de almacenamiento (GCE, Azure, AWS). Esto mejorará los tiempos de respuesta en el aprovicionamiento y será más fácil de administrar.

Con PV necesitamos ejecutar 3 pasos:

1. crear el objeto PV
2. reclamar en PV con un objeto pvc
3. Usar ese pvc mediante un pod que haga uso del volumen

Con storage class solo necesitamos el paso 2 y 3. Storage Class creará de manera utomática en PV.

En lugar de crear el PV por cada volumen se crea un objeto StorageClass y multiples PVC pueden llamar o hacer uso del storage class.

~~~yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google_sc
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
~~~

Diferentes Storage Class puede significar diferente tipo de servicio, diferente proveedor o política de backup.

Del archivo de manifiesto identificamos los siguientes campos:

- **provisioner**: Es el proveedor de servicio que irá creando de manera automática los volumenes. En el siguiente enlace podemos verificar todos los proveedores existente y su parámetros de configuración [Link](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)
- **parameters** Cada proveedor de servicio requerirá diferentes parámetros para la creación de los volúmenes
- **reclaimPolicy** Esta campo, nos indicará que ocurre con los datos cuando un PVC es eliminado.
- **allowVolumeExpansion** Este campo indica si el PVC puede aumentar su capacidad inicialmente configurada

## PVC que consume un StorageClass

Para que un PVC haga uso de un storage clase, se debe especificar en la definición yaml el nombre del storage Class.

~~~yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: miclaim
spec:
  storageClassName: google_sc
  accessModes:
    - ReadWriteOnce
  resources:
     requests:
       storage: 100M
~~~

