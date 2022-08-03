# Labels y selector

Labels y selector son el método estandar que tiene kubernetes para agrupar y filtrar contenedores. Cuando se tiene un cluster con varios pods en ejecución se debe tener cierta habilidad para ir juntándolos y agruparlos de acuerdo a los labels correctos y posterio filtrarlos.

Los labels son etiquetas que describen las propiedades de los pods, rs, deploy y nodos.

Para el scheduling podemos usar los labels y node selector y determinar a que nodo enviar la ejecución de los contenedores.
