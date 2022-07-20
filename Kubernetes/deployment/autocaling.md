# Autoscalado

Los comandos para realizar un escalado de los pods es un forma manual de agregar capacidad de computo a nuestra aplicación. Para que este proceso sea automático hacemos uso de la función de autoescalado. Esta característica permite configurar rango de uso de procesamiento si sobre pasa este límite se crea los pods necesarios.

`kubectl autoscale deployment.v1.apps/nginx-deployment --min=10 --max=15 --cpu-percent=80`

Con este comando estamos diciendo a nuestro deploy que mantenga un mínimo de 3 pods hasta 10 pods con un promedio de cada pod de 80% de uso de CPU.
