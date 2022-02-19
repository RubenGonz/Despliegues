# LAMP en Docker

<div align="center">
    <img src="../Imágenes/LAMP en Docker/Portada.png"/>
</div>

---

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Docker/LAMP%20en%20Docker.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/Docker/LAMP%20en%20Docker.md#requisitos)
- [Creación proyecto ](https://github.com/RubenGonz/Despliegues/blob/main/Docker/LAMP%20en%20Docker.md#creaci%C3%B3n-proyecto)
- [Ejecución y vista final](https://github.com/RubenGonz/Despliegues/blob/main/Docker/LAMP%20en%20Docker.md#ejecuci%C3%B3n-y-vista-final)

---

## Introducción

Una de las estructuras más conocidas y populares de todas es la estructura Lamp. Lo haremos haciendo uso de Docker compose para poder hacer esta instalación sin depender del sistema operativo entre otras variables.

---

## Requisitos

Para este ejercicio haremos uso de las herramientas:

- [Instalación de Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md)
- Instalación de Docker-compose

---

## Creación proyecto 

Para este proyecto vamos a usar una estructura como la siguiente:

<div align="center">
    <img src="../Imágenes/LAMP en Docker/Estructura.png"/>
</div>

Dentro de esta veremos una __carpeta conf__, con la configuración necesaria, una __carpeta dump__, que contendrá la información de nuestra base de datos, la __carpeta www__, que contiene la información sobre nuestra web, el fichero __Dockerfile__ de nuestro apache con php y el fichero __docker-compose.yml__ con la información de nuestro despliegue como ficheros principales. Hablando en detalle:

La carpeta __carpeta dumb__ contendrá el fichero __dbname.sql__ que contendrá:

```
SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
  SET time_zone = "+00:00";

  /*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
  /*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
  /*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
  /*!40101 SET NAMES utf8mb4 */;

  CREATE TABLE `Data` (
    `id` int(11) NOT NULL,
    `name` varchar(20) NOT NULL
  ) ENGINE=InnoDB DEFAULT CHARSET=latin1;

  INSERT INTO `Data` (`id`, `name`) VALUES (1, 'OpenWebinars Article'), (2, 'Crashell'), (3, 'Jerson Martinez'), (4, 'Antonio Moreno');

  /*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
  /*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
  /*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
```

La carpeta __carpeta www__ contendrá el fichero __index.php__ que contendrá:

```
<html>
    <head>
        <title>Welcome to LAMP Infrastructure</title>
        <meta charset="utf-8">
        <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
        <script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
    </head>
    <body>
        <div class="container-fluid">
          <img src="https://manolohidalgo.com/wp-content/uploads/2020/10/fondo-despliegue2.jpg" alt="despliegues app web">
            <?php
                echo "<h1>¡Hola, soy Alumno y te da la bienvenida!</h1>";

                $conn = mysqli_connect('db', 'root', 'test', "dbname");
                $query = 'SELECT * From Data';
                $result = mysqli_query($conn, $query);

                echo '<table class="table table-striped">';
                echo '<thead><tr><th></th><th>id</th><th>name</th></tr></thead>';
                while($value = $result->fetch_array(MYSQLI_ASSOC)){
                    echo '<tr>';
                    echo '<td><a href="#"><span class="glyphicon glyphicon-search"></span></a></td>';
                    foreach($value as $element){
                        echo '<td>' . $element . '</td>';
                    }

                    echo '</tr>';
                }
                echo '</table>';

                $result->close();
                mysqli_close($conn);
            ?>
        </div>
    </body>
</html>
```

El fichero __Dockerfile__ tendrá:

```
FROM php:8.0.0-apache
ARG DEBIAN_FRONTEND=noninteractive
RUN docker-php-ext-install mysqli
RUN apt-get update \
    && apt-get install -y libzip-dev \
    && apt-get install -y zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip

RUN a2enmod rewrite
```

Y el fichero __docker-compose.yml__ contendrá:

```
version: "3.5"
services:
    www:
        build: .
        ports:
            - "5000:80"
        volumes:
            - ./www:/var/www/html
        links:
            - db
        networks:
            - default
    db:
        image: mysql:8.0
        ports:
            - "3306:3306"
        command: --default-authentication-plugin=mysql_native_password
        environment:
            MYSQL_DATABASE: dbname
            MYSQL_USER: root
            MYSQL_PASSWORD: test
            MYSQL_ROOT_PASSWORD: test
        volumes:
            - ./dump:/docker-entrypoint-initdb.d
            - ./conf:/etc/mysql/conf.d
            - persistent:/var/lib/mysql
        networks:
            - default
    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        links:
            - db:db
        ports:
            - 5001:80
        environment:
            MYSQL_USER: root
            MYSQL_PASSWORD: test
            MYSQL_ROOT_PASSWORD: test
volumes:
    persistent:
```

---

## Ejecución y vista final

Para ejecutar nuestro LAMP haremos:

```console
sudo docker-compose up -d
```

Ahora si vamos a los puertos indicados deberíamos ver salidas como la siguiente:

- http://localhost:5000/

<div align="center">
    <img src="../Imágenes/LAMP en Docker/Web.png"/>
</div>

- http://localhost:5001/

<div align="center">
    <img src="../Imágenes/LAMP en Docker/Php.png"/>
</div>

Por lo que con esto ya tendríamos nuestro LAMP desplegado.