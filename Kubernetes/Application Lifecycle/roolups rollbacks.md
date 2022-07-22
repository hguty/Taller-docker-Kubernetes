# Actualizar un deployment (roll out)

Un rollout es la actualización de un deploy y este se llevará a cabo cuando se modifica los campos de template del pod.
Otro cambio como escalado no conlleva una actualización del deploy o roll out.

Para actualizar la imagen de un template de un deploy ejecutamos el comando:

`kubectl set image deployment/nginx-deployment nginx=nginx:1.23.0`

o de manera alternativa podemos editar la configuración del deploy:

`kubectl edit deployment/nginx-deployment`

cambiar la imagen salir y guardar.

Una tercera alternativa es editar el archivo de manifiesto y aplicar los cambios con: `kubectl apply -f <file.yml>`

Una vez aplicado el cambio con cualquiera de las alternativas indicadas podemos ver el proceso del cambio con el comando:

`kubectl rollout status deployment/miapp-deploy`


## Rollback

Cada cambio que hacemos sobre un deploy almacena una número de revisión (versión). Y podemos regresar a una versión anterior.

Ver todas las revisiones disponibles

`kubectl rollout history deployment/myapp-deploy`

Ver el detalle de una revisión específica

 `kubectl rollout history deployment/myapp-deploy --revision=5`

Hacer un rollback a la revisión necesaria

`kubectl rollout undo deployment/myapp-deploy --to-revision=3`

Hacer un rollback a revisión anterior
`kubectl rollout undo deployment/myapp-deploy`

## Escalado de un deploy

Al igual que un replicatioset se puede escalar en la cantidad de réplcias que ejecuta un deploy. El comando es parecido a rs pero difiere en el nombre del objeto que vamos a escalar:

`kubectl scale deployment myapp-deploy --replicas=10`

Se puede editar el manifiesto y cambiar el número de réplicas y aplicar los cambios con `kubectl apply -f <archivo.yaml>`