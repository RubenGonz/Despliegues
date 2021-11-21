# Manipulación de repositorios git

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/Portada.png"/>
</div>

## Índice

- Requisitos
- Configuración
- Creación de un repositorio
- Comprobar el estado de un repositorio
- Realización de commits
- Modificación de un fichero
- Historial

---

## Requisitos

Para poder manipular un repositorio tendremos que disponer con anterioridad de Git en el sistema y para ello puede hacerlo siguiendo los pasos establecidos en la [Instalación de Git](https://github.com/RubenGonz/Despliegues/blob/main/Git/Instalacion%20de%20Git.md).

---

## Configuración

Añadido a los requisitos comentados en la página anteriormente especificada pondremos otra configuración específica, que es usar:

```console
git config --global user.name (nuestro usuario)
git config --global user.email (nuestro correo)
git config --global color.ui auto
```

Una vez hayamos escrito esto en la terminal podremos comprobar si lo hemos puesto correctamente usando el comando:

```console
git config --list
```

Cuya salida si ha salido todo correctamente deberá ser similar a:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/ListarConfiguracion.png"/>
</div>

---

## Creación de un repositorio

Lo primero que haremos tras haber cumplimentado los pasos anteriores será crear nuestro primer repositorio que llamaremos “dpl”.

Para crear un repositorio debemos crear una carpeta donde guardaremos la información en local que será la carpeta con la que trabajaremos, el nombre de esta carpeta será el nombre del repositorio que crearemos. Para crear esta carpeta usaremos el comando:

```console
mkdir (nombre del repositorio)
```

<div align="center">
    <img src="../Imágenes\Manipulación de repositorios\CrearDpl.png"/>
</div>

Después tendremos que meternos dentro de esta carpeta e iniciar el repositorio dentro de esta por lo que haremos:

```console
cd (nombre del repositorio)
git init
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/InicializarRepositorio.png"/>
</div>

El repositorio ya estaría creado y para conocer el contenido dentro de este  podríamos usar muchos comandos, pero uno sencillo es:

```console
ls -la
```

Donde veremos:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/ListarDpl.png"/>
</div>

---

## Comprobar el estado de un repositorio

La manera más sencilla de comprobar el estado de un repositorio es entrando dentro de la carpeta donde se encuentra y usar el comando:

```console
git status
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitStatus1.png"/>
</div>

Ahora mismo nos diría que estamos en la rama predeterminada y que no hay nada preparado para subir al repositorio.

Para mostrar el funcionamiento de los repositorios crearemos un fichero con información usando:

```console
touch indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/CrearIndice.png"/>
</div>

Y lo editaremos usando el comando:

```console
nano indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/NanoIndice.png"/>
</div>

Donde nos aparecerá:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/EditarIndice.png"/>
</div>

Aquí pondremos algo de información como por ejemplo:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/ContenidoIndice1.png"/>
</div>

Para guardar y salir del archivo pulsaremos Ctrl + X, después la letra 'S' y por último a Enter.

Ahora al comprobar el estado del fichero nos saldría:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitStatus2.png"/>
</div>

Y podríamos ver cómo aparece el archivo en el que hemos estado trabajando.

Para poder subirlo al repositorio lo colocaremos en una fase de preparación usando:

```console
git add indice.txt
```

Ahora al ver el estado del fichero veremos esta pantalla:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/AniadirIndice.png"/>
</div>

--- 

## Realización de commits

Este archivo todavía no se encuentra en el repositorio, de momento solo se encuentra en un estado de preparación en local. Para poder subirlo al repositorio local tendremos que usar el comando:

```console
git commit -m “Añadiendo índice de la asignatura de dpl”
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitCommit1.png"/>
</div>

Ahora si lo hemos subido añadiendo además un pequeño mensaje explicando el motivo y contenido del commit. Para comprobar que se ha subido podemos mirar git status y veríamos que el archivo txt ya no está.

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitStatus3.png"/>
</div>

---

## Modificación de un fichero

Ahora estamos en la situación en la que tenemos que modificar un fichero de los que ya hemos subido al repositorio, trabajaremos de vuelta con el fichero indice.txt, así que volveremos a acceder a él y cambiaremos su contenido como en el ejemplo anterior.

```console
nano indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/ModificarIndice.png"/>
</div>

Ya está modificado su contenido y para comprobarlo y saber cuales son los documentos que han cambiado respecto a la última vez podemos usar el comando:

```console
git diff
```

Aquí nos aparecerán los cambios:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitDiff.png"/>
</div>

Para subirlo al repositorio lo haremos como en los ejemplos anteriores:

```console
git add indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitAdd2.png"/>
</div>

```console
git commit -m “Añadiendo los capítulos 3 y 4”
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitCommit2.png"/>
</div>

---

## Historial

Para poder recordar o ver por primera vez los cambios realizados en el último commit efectuado hay un comando que nos muestra en detalle estos cambios:

```console
git show
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitShow.png"/>
</div>

Otra función que podemos usar es cambiar el nombre de commits pasados por ejemplo para cambiar el nombre del commit anterior podemos hacer:

```console
git commit --amend -m “Añadido el capítulo sobre gestión de ramas al índice.”
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitCommit3.png"/>
</div>

Al hacer git show el resultado sería:

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios/GitShow2.png"/>
</div>

Con esto ya podríamos trabajar con nuestros repositorios usando únicamente una terminal.
