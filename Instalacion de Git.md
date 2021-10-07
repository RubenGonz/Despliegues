# Instalación de Git

<center>

![](Imágenes/InstalaciónDeGit/ImagenGit.png)

</center>

***

## Índice de la instalación

- Conocer la versión de git en tu sistema
- Instalación de Git a través del sistema de paquetes
- Instalación de Git a través de la fuente
- Configurar Git

***

## Conocer la versión de git en tu sistema

La forma más sencilla de comprobar la versión actual de git en tu sistema operativo es entrando a una terminal y escribir el comando:

- git --version

Donde nos aparecerá la versión que persiste en tu sistema con información relacionada. Sin embargo si no hay ninguna versión instalada te saldrá un mensaje similar a este:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.002.png)

## Instalación de git a través de paquetes

El sistema de paquetes que nos proporciona sistemas operativos como puede ser ubuntu o linux mint puede ser muy útil en casos como este donde podemos instalar gran cantidad de aplicaciones y sistemas con mucha facilidad con el inconveniente de que no tengamos la última versión disponible del mercado.

Para hacerlo siguiendo este método tendríamos que actualizar nuestro sistema de paquetes abriendo una terminal y escribiendo el comando :

- sudo apt update

Aquí te pedirá la contraseña de administrador donde tras colocarla empezaría el proceso y donde veremos algo similar a lo siguiente:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.003.png)

Una vez terminado lo siguiente sería la propia instalación de git usando los paquetes actualizados con el anterior comando. Para esto el comando que usaremos será:

- sudo apt install git

Tras escribir el comando empezará a cargar y pedirá confirmación, por lo que tendríamos que escribir ‘S’ cuando nos lo pida y tras unos segundos la instalación estaría terminada.

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.004.png)

La manera de comprobar si realmente se ha instalado con éxito es usando el comando comentado en el primer punto del índice donde la salida que veríamos sería similar a:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.005.png)

Instalación de git a través de la fuente

Este método aunque es algo más tedioso que el anterior tiene las ventajas de poder decidir qué versión instalar, no tiene porque ser la establecida en el sistema o la más actual, depende de nuestro criterio y gusto.

Para hacerlo siguiendo este método tendríamos que actualizar nuestro sistema de paquetes abriendo una terminal y escribiendo el comando :

- sudo apt update

Aquí te pedirá la contraseña de administrador donde tras colocarla empezaría el proceso y donde veremos algo similar a lo siguiente:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.003.png)También necesitaremos otro software que se encuentra en el sistema de paquetes para poder instalarnos posteriormente la versión de git elegida por nosotros. Para instalarnos este software solo sería necesario poner en la terminal:

- sudo apt install libz-dev libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext cmake gcc

Que nos daría una salida como esta:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.006.png)

Una vez terminado lo siguiente sería la propia instalación de git, para ello lo primero sería la creación de una carpeta temporal usando:

- mkdir tmp

Y a posteriori entrar en ella usando el comando:

- cd tmp

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.007.png)

Ahora tendríamos que buscar la versión de git que queramos utilizar en nuestro sistema. En este enlace podemos ver todas las versiones que hay actualmente de git:

<https://mirrors.edge.kernel.org/pub/software/scm/git/>![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.008.jpeg)

Una vez elegida la versión que vamos a utilizar tendremos que descargarla y enviar el archivo a git.tar.gz, para ello usaremos la funcionalidad curl de esta manera:

- curl -o git.tar.gz <https://mirrors.edge.kernel.org/pub/software/scm/git/git->(versión elegida).tar.gz

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.009.png)

En el caso de no disponer de la funcionalidad curl se podría instalar poniendo el comando:

- sudo apt  install curl  # version 7.68.0-1ubuntu2.7

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.010.png)

Lo siguiente sería descomprimir el archivo que estamos tratando usando:

- tar -zxf git.tar.gz

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.011.png)

Ahora ya lo tenemos descomprimido y podemos ir al directorio creado que será git-(versión elegida) usando el comando cd.

- cd git-(versión elegida)

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.012.png)

Dentro de esta carpeta lo que tendríamos que hacer es crear e instalar el paquete usando:

- make prefix=/usr/local all
- sudo make prefix=/usr/local install

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.013.png)

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.014.png)

Por último usaremos el comando:

- exec bash

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.015.png)

Y ya tendríamos nuestra versión elegida instalada y funcionando en el sistema y para comprobarlo podemos usar el comando comentado en el primer punto del índice donde la salida que veríamos sería similar a:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.016.png)

Configurar Git

Una vez tengamos Git instalado en nuestro sistema lo siguiente sería configurarlo a nuestro gusto de manera que tendríamos que introducir nuestra identificación para que cuando trabaje lo haga con nuestro usuario.

Para introducir nuestros datos tenemos los comandos:

- git config --global user.name (nuestro usuario)
- git config --global user.email (nuestro correo)

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.017.png)Con estos dos comandos ya nos podríamos validar con git. Para comprobar que lo hemos hecho correctamente podemos usar el comando:

- git config --list

Que nos da la información sobre nuestra actual configuración de Git en el sistema, por ejemplo la salida en mi caso es:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.018.png)

Si con la primera forma no te encuentras cómodo también puedes editar el fichero donde se almacena tu configuración de Git usando uno de los editores de texto que da el sistema como puede ser nano:

- nano ~/.gitconfig

E introducir después los datos correspondientes a tu identidad como en el ejemplo:

![](Aspose.Words.8b029269-607a-4106-a33f-17a63d9e13d6.019.jpeg)

Después tendría que salir de nano usando Ctrl + X, guardar y tendría el Git configurado. Con esto estaría terminada la instalación y configuración de Git en tu sistema.
