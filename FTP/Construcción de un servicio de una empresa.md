# Instalación y administración de servidores de transferencia de archivos en local

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Estructura de archivos]()
- [Docker-compose.yml]()
- [Dominio local]()
- [Comprobación del servicio]()

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

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Script.png"/>
</div>

Donde tendremos la creación de nuestra tabla con unas cuantas inserciones.

Después dentro de nuestra carpeta de dockerfiles tendremos el archivo de configuración de nuestro apache que será el apache.conf que tendrá:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/ApacheConf.png"/>
</div>

Donde le indicaremos información sobre nuestro dominio, algunos alias y redirecciones, la salida del error controlado y el proxy para el uso de PhpMyAdmin y el servicio de sftp.

El el Dockerfile tendremos:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Dockerfile.png"/>
</div>

Que será donde habilitaremos el uso de php en nuestra web y permitiremos el uso del proxy e indicaremos que queremos usar el apache.conf como archivo de configuración.

En nuestra carpeta de www únicamente tendremos una carpeta donde tendremos todas las páginas de errores controlados, una carpeta vacía que será la que actuará de subdominio y la que albergará los archivos del servicio sftp y el archivo index.php que tendrá nuestro home.

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Home.png"/>
</div>

---

## Docker-compose.yml

Por último tendremos nuestro archivo principal, el docker-compose.yml que tendrá:

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Docker-compose.png"/>
</div>

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

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/EtcHosts.png"/>
</div>

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
