# Instalación y administración de servidores de transferencia de archivos en local

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Portada.png"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#requisitos)
- [Estructura de archivos](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#estructura-de-archivos)
- [Docker-compose.yml](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#docker-composeyml)
- [Dominio local](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#dominio-local)
- [Comprobación del servicio](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Construcci%C3%B3n%20de%20un%20servicio%20de%20una%20empresa.md#comprobaci%C3%B3n-del-servicio)

---

## Introducción

En esta ocación lo que haremos será crear un servicio que va a contar con:

- Un dominio principal y un subdominio
- Un servidor de base de datos
- La herramienta PhpMyAdmin
- El soporte de Php
- Un servidor de SFTP

Todo englobado dentro de un contenedor Docker.

Tendremos un dominio principal que será desde donde accederemos a nuestro servicio, dentro de este podremos acceder a nuestro PhpMyAdmin o a nuestro subdominio.

Dentro de PhpMyAdmin tendremos que poder hacer uso de nuestra base de datos y dentro de nuestro subdominio debemos tener acceso a los archivos que hayamos subido a través de SFTP.

---

## Requisitos

Para la instalación y configuración sólo tendremos que contar con Internet, un sistema Ubuntu 20.04. y las herramientas docker-compose y Filezilla.

En el caso de no contar con la herramienta docker-compose solo debemos dirigirnos a la terminal y escribir:

```console
sudo apt update
sudo apt upgrade
sudo apt install docker-compose
```

Y en el caso de no contar con filezilla debemos hacer:

```console
sudo apt update
sudo apt upgrade
sudo apt install -y filezilla
```

---

## Estructura de archivos

Como ya hemos dicho vamos a usar la herramienta docker-compose por lo que tendremos que crear una estructura de ficheros dedicada a nuestro servicio. En nuestro caso será la siguiente:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/EstructuraArchivos.png"/>
</div>

Donde podremos ver que tenemos una carpeta dedicada a nuestra base de datos, una carpeta con los dockerfiles necesarios para nuestro servicio, una carpeta que alberga nuestra página web y el archivo docker-compose.yml.

En nuestro archivo script.sql tendremos la siguiente información:

```console
SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";

CREATE TABLE `Blog` (
  `id` int(11) NOT NULL,
  `name` varchar(20) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

INSERT INTO `Blog` VALUES 
  (1, 'Articulos Java'), 
  (2, 'Articulos Php'), 
  (3, 'Articulos Phyton'), 
  (4, 'Articulos JavaScript');
```

Donde tendremos la creación de nuestra tabla con unas cuantas inserciones.

Después dentro de nuestra carpeta de dockerfiles tendremos el archivo de configuración de nuestro apache que será el apache.conf que tendrá:

```console
<VirtualHost *:80>
    ServerName  rubengrsystem.com
    ServerAlias  www.rubengrsystem.com
    ServerAdmin ruben30303030@gmail.com
    
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    Alias "/errors" "/var/www/html/errors"
    ErrorDocument 404 /errors/error404.html
    
    Alias "/home" "/var/www/html/index.php"
    RedirectMatch ^/$ /home

    ProxyPreserveHost On
    <Proxy *>
        Order allow,deny
        Allow from all
    </Proxy>

    ProxyPass /phpmyadmin/ http://phpmyadmin:80/
    ProxyPassReverse /phpmyadmin/ http://phpmyadmin:80/

    RedirectMatch ^/phpmyadmin$ /phpmyadmin/

    ProxyPass /subdominio/ sftp://localhost:2222/
    ProxyPassReverse /subdominio/ sftp://localhost:2222/
    
    RedirectMatch ^/subdominio$ /subdominio/
</VirtualHost>
```

Donde le indicaremos información sobre nuestro dominio, algunos alias y redirecciones, la salida del error controlado y el proxy para el uso de PhpMyAdmin y el servicio de sftp.

El el Dockerfile tendremos:

```console
FROM php:8.0.0-apache
ARG DEBIAN_FRONTEND=noninteractive
RUN docker-php-ext-install mysqli
RUN apt-get update \
   && apt-get install -y libzip-dev \
   && apt-get install -y zlib1g-dev \
   && rm -rf /var/lib/apt/lists/* \
   && docker-php-ext-install zip
   
RUN a2enmod rewrite
RUN a2dissite 000-default.conf
ADD ./apache.conf /etc/apache2/sites-available
RUN a2ensite apache.conf
RUN a2enmod proxy && a2enmod proxy_http && a2enmod proxy_balancer && a2enmod lbmethod_byrequests
```

Que será donde habilitaremos el uso de php en nuestra web y permitiremos el uso del proxy e indicaremos que queremos usar el apache.conf como archivo de configuración.

En nuestra carpeta de www únicamente tendremos una carpeta donde tendremos todas las páginas de errores controlados, una carpeta vacía que será la que actuará de subdominio y la que albergará los archivos del servicio sftp y el archivo index.php que tendrá nuestro home.

```console
<html>
<head>
    <title>Ruben System</title>
</head>

<body>
    <?php echo '<h1>Pagina home de Rubén González Rodríguez system </h1>'; ?>
</body>
</html>
```

---

## Docker-compose.yml

Por último tendremos nuestro archivo principal, el docker-compose.yml que tendrá:

```
version: "3.7"

services:
    www:
        build: ./dockerfiles/php
        ports:
            - "80:80"
        volumes:
            - ./www:/var/www/html
        links:
            - databases
            - phpmyadmin
            - sftp
        networks:
            - default
    databases:
        image: mysql:8.0
        ports:
            - "3306:3306"
        command: 
            --default-authentication-plugin=mysql_native_password
        environment:
            MYSQL_DATABASE: examen
            MYSQL_USER: test
            MYSQL_PASSWORD: test
        volumes:
            - ./bd:/docker-entrypoint-initdb.d
        networks:
            - default
    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        links:
            - databases:databases
        ports:
            - 8502:80
        environment:
            PMA_HOST: databases
    sftp:
        image: atmoz/sftp
        volumes:
            - ./www/subdominio:/var/www/html/subdominio
        ports:
            - "2222:22"
        command: usuario:123456:2222
```

El primer servicio que tenemos es el de nuestra web donde le indicamos que use el dockerfile para generar su configuración, cual es la ruta raíz de nuestra web y a que está conectada.

En segundo lugar tenemos nuestra base de datos que se genera a partir de la imagen “mysql:8.0”, que tendrá la autenticación nativa de mysql, las variables de acceso a la base de datos y de donde recogemos la información.

Lo siguiente es nuestro PhpMyAdmin que lo obtenemos de la imagen “phpmyadmin/phpmyadmin” y que está conectado a nuestra base de datos.

Por último tenemos nuestro servicio de sftp que lo obtenemos de la imagen “atmoz/sftp” que se comunica usando ./www/subdominio y que tiene el usuario: usuario y la contraseña 123456.

---

## Dominio local

Para acceder a nuestro dominio local deberemos acceder a:

```console
sudo nano /etc/hosts
```

Donde debemos incorporar junto a los alias que tengamos el nombre de nuestro dominio de esta manera:

```console
127.0.0.1       localhost
127.0.1.1       RubenGonz
127.0.0.1       www.rubengrsystem.com rubengrsystem.com rubengrsystem
```

Ahora que ya lo tenemos todo deberemos acceder a nuestro directorio raíz y escribir:

```console
sudo docker-compose up --build
```

Que nos arrancará el docker-compose. 

---

## Comprobación del servicio

Para comprobar que todo está montado deberemos ir a nuestro navegador y escribir:

```console
http://www.rubengrsystem.com
```

Esto nos dirigirá a nuestro home del dominio:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/HomeDominio.png"/>
</div>

Aquí ya podríamos ver que nuestro dominio permite el uso de Php.

Para comprobar la salida de error 404 controlado podremos escribir la ip mal después de nuestro dominio de esta manera:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/ErrorDominio.png"/>
</div>

Para acceder a nuestro phpMyAdmin lo podremos después de nuestro dominio:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/PhpMyAdminDominio.png"/>
</div>

Donde tras escribir nuestras credenciales podríamos acceder y ver nuestra base de datos:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/BdDominio.png"/>
</div>

Por último queda nuestro servicio de Sftp, el cuál no se encuentra disponible debido a problemas con nuestra configuración en el caso de su pleno funcionamiento debería estar accesible a través de:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/SftpDominio.png"/>
</div>

Con esto ya se podría dar por finalizado nuestro servicio.
