# Recursos de un contenedor
El contenedor al ejecutarse en un docker host consummirá los recursos de este. Por lo que si no hay un control sobre los recursos podría afectar el rendimiento o la disponibilidad del sistema

## Ver los recursos que consume un docker

`docker stats nginx-pruebas`

## Limitar los recursos de un docker
### Limitar memoria RAM
-m, --memory="" 	Memory limit (format: <number>[<unit>]). Number is a positive integer. Unit can be one of b, k, m, or g. Minimum is 4m.

`docker run --name nginx-pruebas -d -m "1500mb" -p 8080:80 nginx`

## Limitar el procesador
Necesitamos saber con cuantos cores dispone el docker hots para poder asignar al docker.
Esta información podemos sacar con estos comandos
`more /proc/cpuinfo`
`grep "model name" /proc/cpuinfo`

--cpus=0.000 	Number of CPUs. Number is a fractional number. 0.000 means no limit.
--cpuset-cpus="" 	CPUs in which to allow execution (0-3, 0,1)

`docker run --name nginx-pruebas -d -m "1500mb" --cpuset-cpus 0-2 -p 8080:80 nginx`
