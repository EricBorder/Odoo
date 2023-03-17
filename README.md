# Instalación de Odoo
Para realizar esta actividad hemos montado un servidor de Odoo con una base de datos Postgresql usando un contenedor de Docker.

Lo primero que tenemos que hacer es crear un archivo [docker-compose.yml](https://github.com/EricBorder/Odoo/blob/main/docker-compose.yml)
como el siguiente:
```
version: '3.1'
services:
  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
    ports:
      - "5432:5432"
```

Antes de levantarlo y hacer un docker-compose up comprobamos si el postgresql está utilizando el puerto que queremos con el comando:
``` sudo netstat -putan ```

![1](https://user-images.githubusercontent.com/113973157/225891393-c1554435-8ab4-4bba-85a0-9d3bc208abc8.png)

Podemos ver que el puerto está ocupado por un proceso de postgres asi que no podemos levantar nuestro contenedor.
Tenemos que matar el proceso con: ```service postgresql stop```
Y comprobamos de nuevo con ``` sudo netstat -putan ``` que el proceso ya no nos aparece.

![2](https://user-images.githubusercontent.com/113973157/225891450-5045f0c7-364d-40fb-b581-b816b2f17c80.png)

Ahora procederíamos a levantar nuestro contenedor ```docker-compose up```
y volvemos a comprobar que proceso está utilizando el puerto que necesitamos:

![3](https://user-images.githubusercontent.com/113973157/225891609-ccf678d2-c5cd-45e9-8568-32e4045549a2.png)

De momento solo tendriamos la base de datos en postgres levantada. 
Como la información de la base de datos desaparece cada vez que el contenedor se cierra, hay que crear un volumen para que almacene esa información:
```
    volumes:
      - dam22_postgres:/var/lib/postgresql/data
```
El nombre de nuestro volumen es 'dam22_postgres' y la ruta es donde Postgresql guarda sus datos (por defecto es siempre la misma en todos los equipos), así toda la información de postgresql se guardará en el volumen.
Lo siguiente que tendríamos que hacer es descargar la imagen de Odoo:
``` 
  odoo:
   image: odoo:14.0
   container_name: dam22_odoo
   ports:
     - "8069:8069"
   depends_on:
     - db
```
Damos nombre al contenedor y especificamos los puertos, el puerto por defecto de Odoo es el ```8069```.
Esta es [la guía](https://hub.docker.com/_/odoo)  que hemos utilizado para realizar esta actividad.
