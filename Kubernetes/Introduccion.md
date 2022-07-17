# INTRODUCCION A KUBERNETES

> Kubernetes (K8s) es una plataforma de código abierto para automatizar la implementación, el escalado y la administración de aplicaciones en contenedores. - <https://kubernetes.io/es/>

Código abierto- Suena bien
Automatizar la implementación, que se cree un contenedor, que despligue código y levante un servicio
Automatizar el escalado, si tengo pocas trasacciones necesito pocos contenedores pero si crece las transacciones, debo creceer los contenedores y gestionar ese balanceo de carga
Automatizar la administración de aplicaciones contenedores.

Kubernetes ofrece un entorno de administración centrado en contenedores. Kubernetes orquesta la infraestructura de cómputo, redes y almacenamiento para que las cargas de trabajo de los usuarios no tengan que hacerlo.

CONTENIDO:

3 Kubernetes
    3.1. Conceptos básicos (4 horas)
        i Arquitectura del cluster kubernetes
        ii Contenedores
        iii PODs
        iv Replicaset
        v Deploys
        vi Services
        vii Namespaces
        viii DaemonSet

    3.2. Gestión del ciclo de vida de las aplicaciones (3 horas)
        i Introducción
        ii RollingUp and Rollback
        iii Comandos y argumentos en docker
        iv Comandos y argumentos en kubernetes
        v Variables de entorno
        vi Config maps
        vii Secrets

4 Storage (2 horas)
        i Introducción
        ii Volumes
        iii Persistent volumes
        iv Persistent volumes Claims PVC
        v Pods y PVC

5 Networking (2 horas)
        i Introducción
        ii
6 Scheduling (2 horas)
        i Introducción
        ii Manual scheduling
        iii Labels selectors
        iv Taints y tolerances
        v Node selector, node afnity
        vi Resources y Limits
