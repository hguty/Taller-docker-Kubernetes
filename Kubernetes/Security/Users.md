# Creación de usaurios con certificado

Creamos un directorio donde se almacenaran los certificados.

1. Crear una key para un nuevo usuario
`openssl genrsa -out employee.key 2048`

2. Crear un certifie signed request

   `openssl req -new -key employee.key -out employee.csr -subj "/CN=employee/O=rrhh"`

    **Nota:** Si hay un error en la ejecución de código por un error de rnd hay que editar el archivo /etc/ssl/openssl.cnf y comentar la linea #RANDFILE = $ENV::HOME/.rnd
3. Firmar el csr del usuario con los claves con los datos de ca.crt y ca.key Esto crea el certificado para nuestro usuario
   
   `openssl x509 -req -in employee.csr -CA /var/lib/rancher/k3s/server/tls/client-ca.crt -CAkey /var/lib/rancher/k3s/server/tls/client-ca.key -CAcreateserial -out employee.crt -days 365`

4. Con este certificado ya se puede crear el usaurio del cluster
   `kubectl config set-credentials employee --client-certificate=employee.crt --client-key=employee.key`

   Se puede ver la lista de usuarios del cluster con `kubectl config get-users`
   
5. Ahora creamos un contexto de kubernetes con este usuario y sus credenciales
   `kubectl config set-context employee-context --cluster=default --namespace=espacio2 --user=employee`

    Validamos si se ha creado el contexto el comando:

   `kubectl config get-context`

    Validamos en el contexto en el cual estamos trabajando

   `kubectl config current-context`

   Nos cambiamos al contexto creado

   `kubectl config use-context employee-context`

   Intentamos ejecutar una consulta de pods

   `kubectl get pods`

   Tendremos un error debido a que no se ha configurado aun el rbac