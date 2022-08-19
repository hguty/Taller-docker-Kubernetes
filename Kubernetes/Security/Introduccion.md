# Security en kubernetes

Algunas consideraciones cuando hablamos de seguridad en kuberentes:

La protección de kube-apiserver es fundamental, es la primera linea de defenza del cluster. Deberíamos poder responder las preguntas:

¿Quién tiene acceso? **Authentication**

    - User Id and password
    - Cerificates
    - Proveedores externos de autenticación como LDAP

¿Que pueden hacer dentro del cluster? **Authorization**

- Autorizacion
  - RBAC
- Network Policies

**TLS Certificates**
También se debe proteger las comunicaciones de los elementos dentro de la arquitectura del cluster.

La comunicación interan entre los pods network Police

Autentication



