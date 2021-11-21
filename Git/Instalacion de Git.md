﻿# Instalación de Git

<div align="center">
    <img src="../Imágenes/Instalación de Git/Portada.png"/>
</div>

## Índice de la instalación

- [¿Qué es Git?](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md#qu%C3%A9-es-git)
- [Conocer la versión actual de git en tu sistema](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md#conocer-la-versi%C3%B3n-de-git-en-tu-sistema)
- [Instalación de Git a través del sistema de paquetes](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md#instalaci%C3%B3n-de-git-a-trav%C3%A9s-de-paquetes)
- [Instalación de Git a través de la fuente](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md#instalaci%C3%B3n-de-git-a-trav%C3%A9s-de-la-fuente)
- [Configurar Git](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md#configurar-git)

---

## ¿Qué es Git?

Git es una herramienta muy usada en la actualidad que sirve para gestionar el control de versiones, principalmente, para tratar con código. Este software se emplea para proyectos en los que se trabaja en equipo debido a que es bastante potente, es de software libre, cuenta con un sistema de ramas muy cómodo para el usuario y te da muchas facilidades para trabajar con el versionado.

## Conocer la versión de git en tu sistema

La forma más sencilla de comprobar la versión actual de git en tu sistema operativo es entrar a una terminal y escribir el comando:

```console
git --version
```

La salida de este comando nos enseñará la versión que persiste en nuestro sistema. Sin embargo si no hay ninguna versión instalada te saldrá un mensaje similar a este:

<div align="center">
    <img src="../Imágenes/Instalación de Git/VersionGit.png"/>
</div>

---

## Instalación de git a través de paquetes

El sistema de paquetes que nos proporciona sistemas operativos como puede ser ubuntu o linux mint puede ser muy útil en casos donde no nos importa que versión específica tener. Con estos paquetes podemos instalar gran cantidad de softwares y herramientas con mucha facilidad con el inconveniente de que lo más seguro es que no contemos con la última versión disponible.

Para hacerlo siguiendo este método tendríamos que actualizar nuestro sistema de paquetes para asegurarnos de instalarnos la última versión disponible que nos proporcione el sistema. Para ello abriremos una terminal y escribiremos el comando :

```console
sudo apt update
```

Aquí te pedirá la contraseña de administrador donde tras colocarla y darle a "Enter "empezaría el proceso, dandonos una salida similar a:

<div align="center">
    <img src="../Imágenes/Instalación de Git/ActualizarPaquetes.png"/>
</div>

Una vez este proceso termine lo siguiente sería la propia instalación de git usando los paquetes actualizados. Para iniciar la instalación usaremos:

```console
sudo apt install git
```

Tras escribir el comando empezará a cargar y pedirá confirmación, por lo que tendríamos que escribir "S" cuando nos lo pida y tras unos segundos, la instalación daría comienzo y en poco terminaría.

<div align="center">
    <img src="../Imágenes/Instalación de Git/InstalaciónGit.png"/>
</div>

La manera de comprobar si realmente se ha instalado con éxito es usando el comando comentado en el primer punto:

```console
git --version
```

Si lo hemos ejecutado todo correctamente la salida del comando esta vez sería:

<div align="center">
    <img src="../Imágenes/Instalación de Git/VersionPaquetes.png"/>
</div>

---

## Instalación de git a través de la fuente

El método anterior aunque es mucho más sencillo solo nos deja a nuestra dispocición las versiones que nos de nuestro sistema operativo. Obviamente no siempre querremos estas versiones por lo que si quiesiesemos una versión diferente a estas tendremos que usar este método.

De esta manera tendremos la ventaja de poder decidir qué versión instalar, no tiene porque ser la establecida en el sistema o la más actual simplemente depende de nuestro criterio o gusto.

Para hacerlo siguiendo este método tendremos que actualizar nuestro sistema de paquetes como la anterior vez, abriendo una terminal y escribiendo el comando:

```console
sudo apt update
```

Te volverá a pedir la contraseña de administrador y tras colocarla empezará el proceso y veremos:

<div align="center">
    <img src="../Imágenes/Instalación de Git/ActualizarPaquetes.png"/>
</div>

También necesitaremos otro software que se encuentra en el sistema de paquetes para poder instalarnos posteriormente la versión de git elegida por nosotros. Para instalarnos este software solo sería necesario poner en la terminal el comando:

```console
sudo apt install libz-dev libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext cmake gcc
```

El proceso tendría un aspecto similar a:

<div align="center">
    <img src="../Imágenes/Instalación de Git/SoftwareAdicional.png"/>
</div>

Una vez terminado lo siguiente sería la propia instalación de Git. Para ello lo primero sería la creación de una carpeta temporal usando:

```console
mkdir tmp
```

Y a posteriori entrar en ella usando el comando:

```console
cd tmp
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/CrearCarpetaTmp.png"/>
</div>

Ahora tendremos que buscar la versión de git que queramos instalar en nuestro sistema.

Para ello accederemos a este enlace donde podemos ver todas las versiones que hay actualmente de git:

[Enlace con las versiones disponibles](https://mirrors.edge.kernel.org/pub/software/scm/git/)

<div align="center">
    <img src="../Imágenes/Instalación de Git/WebVersionesGit.png"/>
</div>

Una vez hemos elegido la versión que vamos a utilizar tendremos que descargarla y enviar el archivo a git.tar.gz. Por ello ello usaremos la funcionalidad curl.

En el caso de que no tengamos descargado este software haremos:

```console
sudo apt install curl # version 7.68.0-1ubuntu2.7
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/InstalaciónCurl.png"/>
</div>

Ahora ya podremos bajarnos la version usando el comando:

```console
curl -o git.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-(versión).tar.gz
```

En nuestro caso:

```console
curl -o git.tar.gz https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.29.3.tar.gz
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/InstalaciónGitFuente.png"/>
</div>

Y posteriormente a ello descomprimir el archivo usando:

```console
tar -zxf git.tar.gz
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/DescomprimirArchivo.png"/>
</div>

Ahora que ya lo tenemos descomprimido, podemos ir al directorio creado usando el comando cd.

```console
cd git-(versión elegida)
```

En nuestro caso:

```console
cd git-2.29.3
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/EntrarGit.png"/>
</div>

Dentro de esta carpeta lo que tendríamos que hacer es crear e instalar el paquete usando:

```console
make prefix=/usr/local all
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/CrearPaquete.png"/>
</div>

```console
sudo make prefix=/usr/local install
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/InstalarPaquete.png"/>
</div>

Para terminar nos faltaría usar el comando:

```console
exec bash
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/CrearPaquete.png"/>
</div>

Y ya tendríamos nuestra versión elegida instalada y funcionando en el sistema y para comprobarlo podemos usar el comando comentado en el primer punto del índice donde la salida que veríamos sería similar a:

<div align="center">
    <img src="../Imágenes/Instalación de Git/VersionFuente.png"/>
</div>

---

## Configurar Git

Una vez tengamos Git instalado en nuestro sistema lo siguiente sería configurarlo a nuestro gusto de manera que tendríamos que introducir nuestra identificación para que cuando trabaje lo haga con nuestro usuario.

Para introducir nuestros datos tenemos los comandos:

```console
git config --global user.name (nuestro usuario)
git config --global user.email (nuestro correo)
```

<div align="center">
    <img src="../Imágenes/Instalación de Git/ConfgurarGit.png"/>
</div>

Con estos dos comandos ya nos podríamos validar con git. Para comprobar que lo hemos hecho correctamente podemos usar el comando:

```console
git config --list
```

Que nos da la información sobre nuestra configuración actual de Git en el sistema, por ejemplo la salida en mi caso es:

<div align="center">
    <img src="../Imágenes/Instalación de Git/ConfiguraciónGit.png"/>
</div>

Si con la primera forma no te encuentras cómodo también puedes editar el fichero donde se almacena tu configuración de Git usando uno de los editores de texto que da el sistema como puede ser nano:

```console
nano ~/.gitconfig
```

E introducir después los datos correspondientes a tu identidad como en el ejemplo:

<div align="center">
    <img src="../Imágenes/Instalación de Git/EditarConfiguración.png"/>
</div>

Después tendremos que salir de nano usando Ctrl + X, guardando y tendríamos Git configurado.

Con esto estaría terminada la instalación y configuración de Git en tu sistema.
