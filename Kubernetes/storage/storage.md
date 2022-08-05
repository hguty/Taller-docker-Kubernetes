# Storage

Se conoce que las imágenes y contenedores son efímeros y que la información no es persistente. En docker usamos volumenes para dar la persistencia a los datos y asegurarnos que las aplicaciones puedan almacenar la información que generan y luego recuperarla.

En docker la solución era utilizar volumenes, y esto era montar un directorio del docker host en dentro de contenedor. Sin embargo, para kubernetes se añade una problema: ¿Como administrar la persistencia de los datos en un cluster donde varios nodos deben ver la misma información?

Y finalmente si pensamos que un cluster de kubernetes nos ofrece alta disponibilidad, el almacenamiento del cluster debe "sobrevivir" a una falla del cluster.

Entonces tenemos tres requerimientos para el almacenamiento (storage) en kubernetes:

1. El storage no debe depender del ciclo de vida del pod
2. El storage debe estar disponible para todos los nodos del cluster
3. El storage debe sobrevivir a una falla del cluster.
   
El concepto de persitent volumen solucion los problemas. Pensemos este objeto como un cluster de recursos que es usado para guardar los datos. 

Vamos a necesitar un almacenamiento físico (phisical storage): "localdisk", nfs, cloud storage.


Al igual que otros objetos de kubernetes, persistent volume se crea desde un archivo de manifiesto tipo yaml


Al igual que docker un volumen es un espacio de almacenamiento donde se guardan los datos de manera permanente y se puede acceder a ellos incluso despues que un contenedor se destruye.

Sin embargo; la solución de volumenes para kubernetes debe tener una característica especial y es que por concepto, al ser un cluster de nodos, todos deben ver la misma información (el mismo volumen).

Por ello kubernetes soporta diversos tipos de volumenes e interaciones con proveedores externos (a través de Container Storage Interface): aws ebs, gcp, 


Existen varios tipos de almacenamiento que se puede manejar con kubernetes:

- Persist volume
- Persiste volume claim

## Container Storage Interface

