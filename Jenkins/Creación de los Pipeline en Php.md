## Creación de los Pipeline en Php

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/Portada.png"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Creaci%C3%B3n%20de%20los%20Pipeline%20en%20Php.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Creaci%C3%B3n%20de%20los%20Pipeline%20en%20Php.md#requisitos)
- [Creación del repositorio ](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Creaci%C3%B3n%20de%20los%20Pipeline%20en%20Php.md#creaci%C3%B3n-del-repositorio)
- [Configuración en Jenkins](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Creaci%C3%B3n%20de%20los%20Pipeline%20en%20Php.md#configuraci%C3%B3n-en-jenkins)

---

## Introducción

En esta ocasión lo que haremos será usar un pipeline para comprobar el despliegue de un apache que cuente con php que habremos creado previamente en un repositorio de GitHub. Este contará con una parte donde construiremos nuestra aplicación, una parte donde obtendremos la respuesta de si se ha desplegado o no correctamente y otra donde podremos eliminar o no, depende de nuestro gusto si queremos eliminar el contenedor de la aplicación tras su testeo.

## Requisitos

Para la realización de esta práctica será necesario contar con Jenkins instalado en nuestro sistema. En el caso de no contar con él tienes a tu disposición este enlace que te ayudará con la instalación.

- [Instalación y configuración de Jenkins en Linux](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Linux.md)

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

Donde tendremos una carpeta llamada __src__ que será la que contendrá la estructura de nuestro proyecto. Como esto solo será un proyecto de prueba simplemente crearemos un fichero llamado __index.php__. Este contendrá:

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

Por otra parte y desde la raíz de nuestro proyecto crearemos nuestro __Jenkinsfile__  que será el que contenga nuestro script:

```
pipeline {
    agent any
    stages {
        stage('construccion') {
            steps {
                sh 'docker build -t proyecto-php .'
                sh 'docker run -d --rm -p 8085:80 --name ContenedorPrueba proyecto-php'
            }
        }
        stage('testeo'){
            steps{
                sh 'wget http://localhost:8085/'
            }
        }
//
//      En el caso de querer borrar el contenedor tras testearlo pondremos:
//
//        stage('borarContenedor') {  
//            steps {
//                sh 'docker stop  ContenedorPrueba'  
//            }
//        }
    }
}
```

Y nuestro fichero __Dockerfile__ que será el que tendrá la creación de nuestra imagen:

```
FROM php:7.0-apache
COPY src/ /var/www/html
EXPOSE 80
```

## Configuración en Jenkins

Ahora que ya tenemos nuestro proyecto en el repositorio nos dirigiremos a nuestro Jenkins donde iremos a crearnos un nuevo pipeline.

Opuestamente a lo hecho anteriormente cuando nos dispongamos a colocar nuestro script en la configuración deberemos marcar la casilla de la imagen:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/ConfJenkins.png"/>
</div>

Ahora nos dirigiremos al home del pipeline construido, lo construiriamos y deberíamos tener una imagen similar a:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/SalidaTest.png"/>
</div>

Y en el caso de no haber puesto en el Jenkinsfile la opción de borrar el contenedor tras el test tendríamos una salida como la siguiente en la web donde veríamos que nuestro apache está corriendo y que cuenta con php como se nos pedía:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Php/SalidaWeb.png"/>
</div>