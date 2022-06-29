# Dockerignore
Es un archivo oculto (.dockerignore) dentro del mismo directorio donde está el Dockerfile, en el cual se especifica los archivos o directorios que queremos ignorar en la fase de construcción de la imagem.
Esto con el objetivo de construir imágenes mas ligeras
Se edita el file .dockerignore con el nombre de los archivos o durectorios que queremos que el docker los ignore. Cada archivo o directorio que se quiera ignorar debe ir en una nueva linea

Ejemplo

~~~
# Esto es tratado como comentario 
*/temp* 
*/*/temp*
temp?
~~~

