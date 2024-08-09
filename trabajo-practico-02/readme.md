# Trabajo Práctico 2 - Introducción a Docker

## Desarrollo:

#### 1- Instalar Docker Community Edition 
  - Diferentes opciones para cada sistema operativo
  - https://docs.docker.com/
  - Ejecutar el siguiente comando para comprobar versiones de cliente y demonio.
```bash
docker version
```

![alt text](image.png)

#### 2- Explorar DockerHub
   - Registrase en docker hub: https://hub.docker.com/
   - Familiarizarse con el portal

![alt text](image-1.png)

#### 3- Obtener la imagen BusyBox
  - Ejecutar el siguiente comando, para bajar una imagen de DockerHub
```bash
  docker pull busybox
```

![alt text](image-2.png)

  - Verificar qué versión y tamaño tiene la imagen bajada, obtener una lista de imágenes locales:
```bash
docker images
```

![alt text](image-3.png)

#### 4- Ejecutando contenedores
  - Ejecutar un contenedor utilizando el comando **run** de docker:
```bash
docker run busybox
```

![alt text](image-4.png)

  - Explicar porque no se obtuvo ningún resultado

  - Especificamos algún comando a correr dentro del contendor, ejecutar por ejemplo:
```bash
docker run busybox echo "Hola Mundo"
```

![alt text](image-5.png)

  - Ver los contendores ejecutados utilizando el comando **ps**:
```bash
docker ps
```

![alt text](image-6.png)

  - Vemos que no existe nada en ejecución, correr entonces:
```bash
docker ps -a
```

  - Mostrar el resultado y explicar que se obtuvo como salida del comando anterior.

![alt text](image-7.png)
Muestra todos los contenedores incluso los que no estan andando

#### 5- Ejecutando en modo interactivo

  - Ejecutar el siguiente comando
```bash
docker run -it busybox sh
```
  - Para cada uno de los siguientes comandos dentro de contenedor, mostrar los resultados:

![alt text](image-8.png)

```bash
ps
uptime
free
ls -l /
```

![alt text](image-9.png)

  - Salimos del contendor con:
```bash
exit
```

![alt text](image-10.png)

#### 6- Borrando contendores terminados

  - Obtener la lista de contendores 
```bash
docker ps -a
```

![alt text](image-7.png)

  - Para borrar podemos utilizar el id o el nombre (autogenerado si no se especifica) de contendor que se desee, por ejemplo:
```bash
docker rm elated_lalande
```

![alt text](image-11.png)

  - Para borrar todos los contendores que no estén corriendo, ejecutar cualquiera de los siguientes comandos:
```bash
docker rm $(docker ps -a -q -f status=exited)
```

![alt text](image-12.png)

```bash
docker container prune
```

![alt text](image-13.png)

#### 7- Construir una imagen

- A partir del código https://github.com/ingsoft3ucc/SimpleWebAPI crearemos una imagen.
- Clonar repo

![alt text](image-14.png)

- Crear imagen etiquetándola con un nombre. El punto final le indica a Docker que use el dir actual
```bash
docker build –t mywebapi .
```

![alt text](image-15.png)

- Revisar Dockerfile y explicar cada línea
![alt text](image-16.png)
- Ver imágenes disponibles
![alt text](image-17.png)
- Ejecutar un contenedor con nuestra imagen
![alt text](image-18.png)
- Subir imagen a nuestra cuenta de dockerhub

#### 8- Publicando puertos

En el caso de aplicaciones web o base de datos donde se interactúa con estas aplicaciones a través de un puerto al cual hay que acceder, estos puertos están visibles solo dentro del contenedor. Si queremos acceder desde el exterior deberemos exponerlos.

  - Ejecutar la siguiente imagen, en este caso utilizamos la bandera -d (detach) para que nos devuelva el control de la consola:

```bash
docker run --name myapi -d mywebapi
```
  - Ejecutamos un comando ps:
  - Vemos que el contendor expone 3 puertos el 80, el 5254 y el 443, pero si intentamos en un navegador acceder a http://localhost/WeatherForecast no sucede nada.

  - Procedemos entonces a parar y remover este contenedor:
```bash
docker kill myapi
docker rm myapi
```
  - Vamos a volver a correrlo otra vez, pero publicando los puertos 80 y 5254

```bash
docker run --name myapi -d -p 80:80 -p 5254:5254 mywebapi
```
  - Accedamos nuevamente a http://localhost/WeatherForecast y a http://localhost/swagger/index.html y expliquemos que sucede.

#### 9- Modificar Dockerfile para soportar bash 

- Modificamos dockerfile para que entre en bash sin ejecutar automaticamente la app

 
```bash
#ENTRYPOINT ["dotnet", "SimpleWebAPI.dll"]
CMD ["/bin/bash"]
```
- Rehacemos la imagen
- Corremos contenedor en modo interactivo exponiendo puerto
```
docker run -it --rm -p 80:80 mywebapi
```
- Navegamos a http://localhost/weatherforecast
- Vemos que no se ejecuta automaticamente
- Ejecutamos app:
```
dotnet SimpleWebAPI.dll
```
-Volvemos a navegar a http://localhost/weatherforecast
- Salimos del contenedor


  
#### 10- Montando volúmenes

Hasta este punto los contenedores ejecutados no tenían contacto con el exterior, ellos corrían en su propio entorno hasta que terminaran su ejecución. Ahora veremos cómo montar un volumen dentro del contenedor para visualizar por ejemplo archivos del sistema huésped:

  - Ejecutar el siguiente comando, cambiar myusuario por el usuario que corresponda. En Mac puede utilizarse /Users/miusuario/temp):
```bash
docker run -it --rm -p 80:80 -v /Users/miuser/temp:/var/temp  mywebapi
```
  - Dentro del contenedor correr
```bash
ls -l /var/temp
touch /var/temp/hola.txt
```
  - Verificar que el Archivo se ha creado en el directorio del guest y del host.

#### 11- Utilizando una base de datos
- Levantar una base de datos PostgreSQL

```bash
mkdir $HOME/.postgres

docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -v $HOME/.postgres:/var/lib/postgresql/data -p 5432:5432 -d postgres:9.4
```
- Ejecutar sentencias utilizando esta instancia

```bash
docker exec -it my-postgres /bin/bash

psql -h localhost -U postgres

#Estos comandos se corren una vez conectados a la base

\l
create database test;
\connect test
create table tabla_a (mensaje varchar(50));
insert into tabla_a (mensaje) values('Hola mundo!');
select * from tabla_a;

\q

exit
```

- Conectarse a la base utilizando alguna IDE (Dbeaver - https://dbeaver.io/, eclipse, IntelliJ, etc...). Interactuar con los objectos objectos creados.

- Explicar que se logro con el comando `docker run` y `docker exec` ejecutados en este ejercicio.

#### 12- Hacer el punto 11 con Microsoft SQL Server
- Armar un contenedor con SQL Server
- Crear BD, Tablas y ejecutar SELECT
