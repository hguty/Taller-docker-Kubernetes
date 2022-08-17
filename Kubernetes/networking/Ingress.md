# Ingress

Problemas/retos que tengo para el acceso de mis usuarios a una aplicación hosteada en un cluster de kubernetes:

- En un cluster on-premise no tengo un servicio de tipo loadbalancer
- En un hosting de nube necesito tantos loadbalancer como aplicaciones quiere desplegra
- Por cada loadbalancer es una nueva ip pública
- La gestión de certificados SSL para la https
  
Ingress es un objeto del API de kubernetes que gestiona los accesos a los servicios del cluster, generalmente acceso de tipo http y https. En resumen es un objeto que ayuda a que los usuarios finales accedan a las aplicaciones a través de un conjunto de reglas.

Las funcionalidades que podemos obtener con Ingress son load balancer, SSL y virtual hosting.

Ingress expone HTTP y HTTPS para usuarios finales, mientras se comunica internamente con los servicios del cluster kubernetes.

![Ingress Controller](../img/IngressController.png)

**Ingress resources** son configuraciones y las reglas de routing requeridas para exponer y administrar el ingreso de las solicitudes de usuario.

Un **Ingress controller** son las herramientas responsables del funcionamiento (fulfilling) del Ingress, generalmente con load balancer, a través de la configuración de edge router o frontends adicionales para ayudar a manejar el tráfico.

![Ingress Controller](../img/IngressControllerexp.png)

Un ejemplo de configuración de Ingress sería:

~~~yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: minimal-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /testpath
        pathType: Prefix
        backend:
          service:
            name: test
            port:
              number: 80
~~~

Se identifica los campos apiVersion, kind, metadata y spec al igual que la mayoría de objetos en kubernetes

El nombre de Ingress debe ser valido para subdominio DNS. 

Ingress usan de manera frecuente **annotations** para configurar algunas opciones dependiendo del Ingress controller. Diferentes Ingress contollers soportan diferentes annotatioms, habrá que revisar la documentación dependiendo del Controler que se va a utilizar.

Spec es la sección de la configuración en donde se establecen las características del objeto, y si funcionará como balanceo de carga o reverse procy.
Mas importante spec, contiene la reglas de incoming request. Ingress solo soporta reglas de redirección HTTP(s)

## Ingress rules

Cada regla HTTP contiene la siguiente información:

- Un host opcional. En el ejemplo, no se ha espcificado este host, 

    An optional host. In this example, no host is specified, so the rule applies to all inbound HTTP traffic through the IP address specified. If a host is provided (for example, foo.bar.com), the rules apply to that host.
    A list of paths (for example, /testpath), each of which has an associated backend defined with a service.name and a service.port.name or service.port.number. Both the host and path must match the content of an incoming request before the load balancer directs traffic to the referenced Service.
    A backend is a combination of Service and port names as described in the Service doc or a custom resource backend by way of a CRD. HTTP (and HTTPS) requests to the Ingress that matches the host and path of the rule are sent to the listed backend.

A defaultBackend is often configured in an Ingress controller to service any requests that do not match a path in the spec.
