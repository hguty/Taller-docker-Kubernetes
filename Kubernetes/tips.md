# Tips

Para el examen de certificación puede ser una complicación recordar cada campo del amnifiesto de los objetos en kubernetes por lo que podrían usar el comando kubectl run y generar automáticamente los archivos yml.

Te dejamos algunos ejemplos:

<https://kubernetes.io/docs/reference/kubectl/conventions/>

Crear un pod nginx desde la linea de comandos 

`kubectl run nginx --image=nginx`

A partir de este comando se puede generar el manifest yaml con la opción (-o yaml). Y para que solo genere el archivo sin crear el obejto (--dry-run)

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

Crear un deploy.

`kubectl create deployment --image=nginx nginx`

Generar el archivo manifiesto del deploy en formato YAML (-o yaml). Sin crear el objeto (--dry-run)

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml`

Generando el archivo y guardando en el directorio actual:

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

Grabar el archivo y editarlo de acuerdo a las necesidades del deploy.

Generando el archivo de deploy con 4 réplicas  y guardando en el directorio actual:
`kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`