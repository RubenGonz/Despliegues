## Creación de los Pipeline en Php

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

---

## Introducción

En esta ocación lo que haremos será usar 

## Requisitos

Para la realización de esta práctica será necesario contar con Jenkins instalado en nuestro sistema. En el caso de no contar con ella tienes a tu dispocición estos enlaces que te ayudarán con la instalación.

- [Instalación y configuración de Jenkins en Linux](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Linux.md)
- [Instalación y configuración de Jenkins en Docker](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Docker.md)

Y a poder ser tener un manejo básico de como crear un pipeline en Jenkins, en su defecto puedes contar con este informe que te ayudará:

- [Fundamentos de un Pipeline Jenkins](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Fundamentos%20de%20un%20Pipeline%20Jenkins.md)

## Creación del repositorio 

Como ya hemos dicho lo que vamos a hacer esta vez es usar un proyecto que tengamos alojado en GitHub para ejecutar el pipeline. Por ello vamos a crear un proyecto de prueba que llamaremos:

```
proyecto-php
```

Dentro de aqui crearemos una estructura de ficheros como la siguiente:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/EstructuraFicheros.png"/>
</div>

Donde tendremos una carpeta llamada __src__ que será la que contendrá la estructura de nuestro proyecto. Como esto solo será un proyecto de prueba simplemente crearemos un fichero llamado __index-php__. Este contendrá:

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Index de nuestro proyecto</title>
</head>

<body>
    <?php 
        echo "<p>Texto escrito en PHP por Rubén González Rodríguez</p>"; 
    ?>
</body>

</html>
```

Por otra parte y desde la raiz de nuestro proyecto crearemos nuestro __Jenkinfile__  que será el que contenga nuestro script:

```

```

## Configuración en Jenkins

Ahora que ya tenemos nuestro proyecto en el repositorio nos dirigiremos a nuestro Jenkins donde iriamos a crearnos un nuevo pipeline.

Opuestamente a lo hecho anteriormente cuando nos dispongamos a colocar nuestro script en la configuración deberemos marcar la casilla de la imagen:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/ConfJenkins.png"/>
</div>