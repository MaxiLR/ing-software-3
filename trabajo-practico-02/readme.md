# Trabajo Práctico 2 - Introducción a Docker

## Desarrollo:

#### 1- Instalar Docker Community Edition

- Diferentes opciones para cada sistema operativo
- https://docs.docker.com/
- Ejecutar el siguiente comando para comprobar versiones de cliente y demonio.

  ```bash
  docker version
  ```

  <img src="./images/image.png" width="500"/>

#### 2- Explorar DockerHub

- Registrase en docker hub: https://hub.docker.com/
- Familiarizarse con el portal  
  <img src="./images/image-1.png" width="500"/>

#### 3- Obtener la images/imagen BusyBox

- Ejecutar el siguiente comando, para bajar una images/imagen de DockerHub

  ```bash
    docker pull busybox
  ```

  <img src="./images/image-2.png" width="500"/>

- Verificar qué versión y tamaño tiene la images/imagen bajada, obtener una lista de imágenes locales:

  ```bash
  docker images/images
  ```

  <img src="./images/image-3.png" width="500"/>

#### 4- Ejecutando contenedores

- Ejecutar un contenedor utilizando el comando **run** de docker:

  ```bash
  docker run busybox
  ```

  <img src="./images/image-4.png" width="500"/>

- Explicar porque no se obtuvo ningún resultado

- Especificamos algún comando a correr dentro del contendor, ejecutar por ejemplo:

  ```bash
  docker run busybox echo "Hola Mundo"
  ```

  <img src="./images/image-5.png" width="500"/>

- Ver los contendores ejecutados utilizando el comando **ps**:

  ```bash
  docker ps
  ```

  <img src="./images/image-6.png" width="500"/>

- Vemos que no existe nada en ejecución, correr entonces:

  ```bash
  docker ps -a
  ```

- Mostrar el resultado y explicar que se obtuvo como salida del comando anterior.
  <img src="./images/image-7.png" width="500"/>  
  _Muestra todos los contenedores incluso los que no estan andando_

#### 5- Ejecutando en modo interactivo

- Ejecutar el siguiente comando

  ```bash
  docker run -it busybox sh
  ```

- Para cada uno de los siguientes comandos dentro de contenedor, mostrar los resultados:
  <img src="./images/image-8.png" width="500"/>

  ```bash
  ps
  uptime
  free
  ls -l /
  ```

  <img src="./images/image-9.png" width="500"/>

- Salimos del contendor con:

  ```bash
  exit
  ```

  <img src="./images/image-10.png" width="500"/>

#### 6- Borrando contendores terminados

- Obtener la lista de contendores

  ```bash
  docker ps -a
  ```

  <img src="./images/image-7.png" width="500"/>

- Para borrar podemos utilizar el id o el nombre (autogenerado si no se especifica) de contendor que se desee, por ejemplo:

  ```bash
  docker rm elated_lalande
  ```

  <img src="./images/image-11.png" width="500"/>

- Para borrar todos los contendores que no estén corriendo, ejecutar cualquiera de los siguientes comandos:

  ```bash
  docker rm $(docker ps -a -q -f status=exited)
  ```

    <img src="./images/image-12.png" width="500"/>

  ```bash
  docker container prune
  ```

  <img src="./images/image-13.png" width="500"/>

#### 7- Construir una images/imagen
- Conceptos de DockerFile
  - Leer https://docs.docker.com/engine/reference/builder/ 
  - Describir las instrucciones
    - FROM: _Define la imagen base a partir de la cual se construye la nueva imagen._
    - RUN: _Ejecuta un comando en la imagen y guarda el resultado en una nueva capa._
    - ADD: _Copia archivos/directorios al contenedor, con descompresión automática de archivos._
    - COPY: _Copia archivos/directorios al contenedor sin descompresión._
    - EXPOSE: _Documenta los puertos en los que la aplicación escucha._
    - CMD: _Especifica el comando predeterminado que se ejecuta al iniciar el contenedor, que puede ser sobreescrito._
    - ENTRYPOINT: _Define un comando que siempre se ejecuta al iniciar el contenedor._
- A partir del código https://github.com/ingsoft3ucc/SimpleWebAPI crearemos una images/imagen.
- Clonar repo
  <img src="./images/image-14.png" width="500"/>

- Crear images/imagen etiquetándola con un nombre. El punto final le indica a Docker que use el dir actual

  ```bash
  docker build –t mywebapi .
  ```

  <img src="./images/image-15.png" width="500"/>

- Revisar Dockerfile y explicar cada línea  
  <img src="./images/image-16.png" width="500"/>
- Ver imágenes disponibles  
  <img src="./images/image-17.png" width="500"/>
- Ejecutar un contenedor con nuestra images/imagen  
  <img src="./images/image-18.png" width="500"/>
- Subir images/imagen a nuestra cuenta de dockerhub  
  <img src="./images/image-19.png" width="500"/>

#### 8- Publicando puertos

En el caso de aplicaciones web o base de datos donde se interactúa con estas aplicaciones a través de un puerto al cual hay que acceder, estos puertos están visibles solo dentro del contenedor. Si queremos acceder desde el exterior deberemos exponerlos.

- Ejecutar la siguiente images/imagen, en este caso utilizamos la bandera -d (detach) para que nos devuelva el control de la consola:

  ```bash
  docker run --name myapi -d mywebapi
  ```

  <img src="./images/image-20.png" width="500"/>

- Ejecutamos un comando ps:
- Vemos que el contendor expone 3 puertos el 80, el 5254 y el 443, pero si intentamos en un navegador acceder a http://localhost/WeatherForecast no sucede nada.

  <img src="./images/image-21.png" width="500"/>

- Procedemos entonces a parar y remover este contenedor:

  ```bash
  docker kill myapi
  docker rm myapi
  ```

  <img src="./images/image-22.png" width="500"/>

- Vamos a volver a correrlo otra vez, pero publicando los puertos 80 y 5254

  ```bash
  docker run --name myapi -d -p 80:80 -p 5254:5254 mywebapi
  ```

  <img src="./images/image-23.png" width="500"/>

- Accedamos nuevamente a http://localhost/WeatherForecast y a http://localhost/swagger/index.html y expliquemos que sucede.
  <img src="./images/image-24.png" width="500"/>  
  _Ahora los puertos del contenedor se encuentrar expuestos/mapeados a los mismos puertos pero de la maquina host, lo que nos permite accederlos gracias a la redireccion de puertos que se hace al realizar este mapeo._

#### 9- Modificar Dockerfile para soportar bash

- Modificamos dockerfile para que entre en bash sin ejecutar automaticamente la app

  ```bash
  # ENTRYPOINT ["dotnet", "SimpleWebAPI.dll"]
  CMD ["/bin/bash"]
  ```

  <img src="./images/image-25.png" width="500"/>

- Rehacemos la images/imagen
  <img src="./images/image-26.png" width="500"/>

- Corremos contenedor en modo interactivo exponiendo puerto

  ```bash
  docker run -it --rm -p 80:80 mywebapi
  ```

  <img src="./images/image-27.png" width="500"/>

- Navegamos a http://localhost/weatherforecast
- Vemos que no se ejecuta automaticamente
  <img src="./images/image-28.png" width="500"/>

- Ejecutamos app:

  ```bash
  dotnet SimpleWebAPI.dll
  ```

  <img src="./images/image-29.png" width="500"/>

-Volvemos a navegar a http://localhost/weatherforecast
<img src="./images/image-30.png" width="500"/>

- Salimos del contenedor

#### 10- Montando volúmenes

Hasta este punto los contenedores ejecutados no tenían contacto con el exterior, ellos corrían en su propio entorno hasta que terminaran su ejecución. Ahora veremos cómo montar un volumen dentro del contenedor para visualizar por ejemplo archivos del sistema huésped:

- Ejecutar el siguiente comando, cambiar myusuario por el usuario que corresponda. En Mac puede utilizarse /Users/miusuario/temp):

  ```bash
  docker run -it --rm -p 80:80 -v /Users/miuser/temp:/var/tmp  mywebapi
  ```

  <img src="./images/image-32.png" width="500"/>

- Dentro del contenedor correr

  ```bash
  ls -l /var/tmp
  touch /var/tmp/hola.txt
  ```

  <img src="./images/image-31.png" width="500"/>

- Verificar que el Archivo se ha creado en el directorio del guest y del host.  
  <img src="./images/image-31b.png" width="500"/>

#### 11- Utilizando una base de datos

- Levantar una base de datos PostgreSQL

  ```bash
  mkdir $HOME/.postgres

  docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -v $HOME/.postgres:/var/lib/postgresql/data -p 5432:5432 -d postgres:9.4
  ```

  <img src="./images/image-33.png" width="500"/>

- Ejecutar sentencias utilizando esta instancia

  ```bash
  docker exec -it my-postgres /bin/bash

  psql -h localhost -U postgres
  ```

    <img src="./images/image-34.png" width="500"/>

  ```bash
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

  <img src="./images/image-35.png" width="500"/>

- Conectarse a la base utilizando alguna IDE (Dbeaver - https://dbeaver.io/, eclipse, IntelliJ, etc...). Interactuar con los objetos creados.
  <img src="./images/image-36.png" width="500"/>
  <img src="./images/image-37.png" width="500"/>

- Explicar que se logro con el comando `docker run` y `docker exec` ejecutados en este ejercicio.  
  _Con el `docker run` se levanto un contenedor con la images/imagen del servidor de la base de datos, en este caso PostgreSQL, se exporto una variable del sistema **POSTGRES_PASSWORD** que es la password de la base de datos, se creo un volumen para mapear directorio entre las maquinas guest y host, se mapearon los puerto 5432 de ambas y se ejecuto en modo detach para recuperar el acceso a la consola y no quedarse viendo los logs del contenedor.
  Con el `docker exec` abro una terminal dentro del contenedor que ya esta corriendo para tener acceso al mismo desde dentro y poder, si se quisiera, hablar con la base de datos para hacer consultas, como en este caso._

#### 12- Hacer el punto 11 con Microsoft SQL Server

- Armar un contenedor con SQL Server  
  <img src="./images/image-39.png" width="500"/>
- Crear BD, Tablas y ejecutar SELECT  
  <img src="./images/image-38.png" width="500"/>  
  <img src="./images/image-40.png" width="500"/>  
  <img src="./images/image-41.png" width="500"/>  
  <img src="./images/image-42.png" width="500"/>
