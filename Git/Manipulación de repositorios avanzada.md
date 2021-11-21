# Ejercicios en Git

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/Portada.png"/>
</div>

## Índice

- Requisitos
- Ejercicio 1
- Ejercicio 2
- Ejercicio 3
- Ejercicio 4
- Ejercicio 5
- Ejercicio 6
- Ejercicio 7
- Ejercicio 8
- Ejercicio 9

---

## Requisitos

Para poder hacer los siguientes nueve ejercicios sobre el tratamiento de Git será necesario contar con el repositorio:

- [Repositorio de un libro](https://github.com/jpexposito/libro)

Para poder usar este repositorio en el caso de que no lo tengamos tendremos que clonarlo. Para clonar un repositorio simplemente tendremos que hacer:

```console
git clone https://github.com/jpexposito/libro.git
```

Ahora que ya contamos con el repositorio libro en local entraremos a la carpeta donde lo hemos clonado usando:

```console
cd libro
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitClone.png"/>
</div>

---

## Ejercicio 1

En este ejercicio tendremos que:

~~~
- Mostrar el historial de cambios del repositorio.
- Crear la carpeta capítulos y crear dentro de ella el fichero capitulo1.txt con el siguiente texto.

"Git es un sistema de control de versiones ideado por Linus Torvalds."

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit de los cambios con el mensaje "Añadido capítulo 1."
- Volver a mostrar el historial de cambios del repositorio.
~~~

Por ello el primer paso será obtener el historial de cambios usando el comando:

```console
git log
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog1.png"/>
</div>

Para crear cualquier tipo de carpeta usaremos el comando "mkdir" seguido del nombre que le queremos dar a la carpeta. Por ello nosotros tendremos que hacer:

```console
mkdir capitulos
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearCapitulos.png"/>
</div>

Lo siguiente que se nos pide es crear un fichero dentro de esta carpeta así que entraremos dentro de esta carpeta usando el comando cd y la crearemos usando touch. Los comandos usados finalmente serían:

```console
cd capitulos
touch capitulo1.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearCapitulo1.png"/>
</div>

Una vez el archivo está creado nos pide que lo editemos poniendo un texto, por lo que usaremos un editor de texto como por ejemplo nano para poder hacerlo. El comando sería:

```console
nano capitulo1.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoCapitulo1.png"/>
</div>

Dentro del editor de texto colocaremos el texto especificado que es:

*Git es un sistema de control de versiones ideado por Linus Torvalds.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/TextoCapitulo1.png"/>
</div>

Para salir de nano solo tendríamos que usar Ctrl + X, después a la letra 'S' y por último a Enter.

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Para ello haremos:

```console
git add .
git commit -m "Añadido capitulo 1"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit1.png"/>
</div>

Ahora que ya está el repositorio podremos ver nuestro historial para comprobar los cambios usando nuevamente:

```console
git log
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog2.png"/>
</div>

---

## Ejercicio 2

En este ejercicio tendremos que:

~~~
- Crear el fichero capitulo2.txt en la carpeta capítulos con el siguiente texto.

El flujo de trabajo básico con Git consiste en: 1- Hacer cambios en el repositorio. 2- Añadir los cambios a la zona de intercambio temporal. 3- Hacer un commit de los cambios.

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit de los cambios con el mensaje Añadido capítulo 2.
- Mostrar las diferencias entre la última versión y las dos versiones anteriores.
~~~

Para este segundo ejercicio crearemos un fichero como en el primer ejercicio, siendo esta vez el siguiente capítulo:

```console
touch capitulo2.txt
nano capitulo2.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearCapitulo2.png"/>
</div>

Lo siguiente será poner el texto que nos especifica:

*El flujo de trabajo básico con Git consiste en:*

*1- Hacer cambios en el repositorio.*

*2- Añadir los cambios a la zona de intercambio temporal.*

*3- Hacer un commit de los cambios.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoCapitulo2.png"/>
</div>

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Por ello haremos:

```console
git add .
git commit -m "Añadido capitulo 2"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit2.png"/>
</div>

Para poder ver las diferencias entre diferentes versiones podemos usar el comando "git diff", por ejemplo si quisiéramos ver la diferencia entre la versión más actual y la antepenúltima, es decir, dos versiones anteriores a esta, se usaría:

```console
git diff HEAD~2..HEAD
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitDiff1.png"/>
</div>

---

## Ejercicio 3

En este ejercicio tendremos que:

~~~
- Crear el fichero capitulo3.txt en la carpeta capítulos con el siguiente texto.

Git permite la creación de ramas lo que permite tener distintas versiones del mismo proyecto y trabajar de manera simultánea en ellas.

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit de los cambios con el mensaje Añadido capítulo 3.
- Mostrar las diferencias entre la primera y la última versión del repositorio.
~~~

Para este ejercicio crearemos un fichero como en el primer ejercicio, siendo esta vez el siguiente capítulo:

```console
touch capitulo3.txt
nano capitulo3.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearCapitulo3.png"/>
</div>

Lo siguiente será poner el texto que nos especifica:

*Git permite la creación de ramas lo que permite tener distintas versiones del mismo proyecto y trabajar de manera simultánea en ellas.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoCapitulo3.png"/>
</div>

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Por ello haremos:

```console
git add .
git commit -m "Añadido capitulo 3"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit3.png"/>
</div>

Para poder ver las diferencias entre diferentes versiones usaremos el comando "git log", por ejemplo si quisiéramos ver la diferencia entre la primera versión y la actual debemos hacer:

```console
git log
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog3.png"/>
</div>

Con esto ya tendremos el identificador de nuestra primera versión y la podremos comparar con la actual, en este caso es:

```console
git diff 4dcb74b18a32f24061bc2e7c415f09f7aaff4971..HEAD
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitDiff2.png"/>
</div>

---

## Ejercicio 4

En este ejercicio tendremos que:

~~~
- Crea el fichero índice.txt la siguiente línea:

Índice de los capítulos, con conceptos avanzados de git

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit de los cambios con el mensaje "Indice de los capítulos, con conceptos avanzados de git.
- Mostrar quién ha hecho cambios sobre el fichero indice.txt.
~~~

Para este ejercicio crearemos un fichero como en el primer ejercicio, siendo esta vez el índice:

```console
touch indice.txt
nano indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearIndice.png"/>
</div>

Lo siguiente será poner el texto que nos especifica:

*Índice de los capítulos, con conceptos avanzados de Git*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoIndice.png"/>
</div>

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Por ello haremos:

```console
git add .
git commit -m "Indice de los capitulos, con conceptos avanzados de git"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit4.png"/>
</div>

Lo siguiente que nos pide es saber quién ha hecho cambios sobre el fichero indice.txt por lo que usaremos el comando "git annote":

```console
git annote indice.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitAnnote.png"/>
</div>

---

## Ejercicio 5

En este ejercicio tendremos que:

~~~
- Crear una nueva rama bibliografía y mostrar las ramas del repositorio.
~~~

Para ello lo primero que haremos será la rama usando el comando:

```console
git branch bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitBranch1.png"/>
</div>

Con esto la rama ya estaría creada y para comprobarlo podemos mirar las ramas actuales usando el comando:

```console
git branch -av
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitBranch2.png"/>
</div>

---

## Ejercicio 6

En este ejercicio tendremos que:

~~~
- Crear el fichero capitulos/capitulo4.txt y añadir el texto siguiente:

En este capítulo veremos cómo usar GitHub para alojar repositorios en remoto.

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit con el mensaje "Añadido capítulo 4."
- Mostrar la historia del repositorio incluyendo todas las ramas.
~~~

Para este ejercicio crearemos un fichero como en el primer ejercicio, siendo el siguiente capítulo:

```console
touch capitulo4.txt
nano capitulo4.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearCapitulo4.png"/>
</div>

Lo siguiente será poner el texto que nos especifica:

*En este capítulo veremos como usar GitHub para alojar repositorios en remoto.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoCapitulo4.png"/>
</div>

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Por ello haremos:

```console
git add .
git commit -m "Añadido capitulo 4"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit5.png"/>
</div>

Ahora si queremos comprobar la historia en todas las ramas podemos hacer:

```console
git log --graph --all --oneline
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog4.png"/>
</div>

---

## Ejercicio 7

En este ejercicio tendremos que:

~~~
- Cambiar a la rama bibliografía.
- Crear el fichero bibliografia.txt y añadir la siguiente referencia:

Chacon, S. and Straub, B. Pro Git. Apress.

- Añadir los cambios a la zona de intercambio temporal.
- Hacer un commit con el mensaje "Añadida primera referencia bibliográfica."
- Mostrar la historia del repositorio incluyendo todas las ramas.
~~~

Esta vez trabajaremos sobre la rama bibliografía por lo que usaremos el comando checkout de esta manera:

```console
git checkout bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCheckout1.png"/>
</div>

Para este ejercicio crearemos un fichero como en el primer ejercicio, siendo esta vez bibliografía.txt de esta manera:

```console
touch bibliografía.txt
nano bibliografía.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/CrearBibliografia.png"/>
</div>

Lo siguiente será poner el texto que nos especifica:

*Chacon, S. and Straub, B. Pro Git. Apress.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia1.png"/>
</div>

Ahora que ya hemos hecho los cambios queremos subirlo al repositorio local por lo que tendremos que pasarlo por el estado de preparación y después hacer el commit con el mensaje correspondiente. Por ello haremos:

```console
git add .
git commit -m "Añadida primera referencia bibliografica"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit6.png"/>
</div>

Ahora si queremos comprobar la historia en todas las ramas podemos hacer:

```console
git log --graph --all --oneline
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog5.png"/>
</div>

---

## Ejercicio 8

En este ejercicio tendremos que:

~~~
- Fusionar la rama bibliografía con la rama main.
- Mostrar la historia del repositorio incluyendo todas las ramas.
- Eliminar la rama bibliografía.
- Mostrar de nuevo la historia del repositorio incluyendo todas las ramas.
~~~

Ahora que hemos trabajo en la rama "bibliografía" tenemos que añadir los progresos en la rama "main" que lo haremos primero yendo a la rama "main" y después usando el comando git merge de esta manera:

```console
git checkout main
git merge bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitMerge1.png"/>
</div>

Ahora para mostrar el historial de todas las ramas podríamos emplear este comando:

```console
git log --graph --all --oneline
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog6.png"/>
</div>

Ahora que ya hemos usado esta rama y hemos terminado con ella lo correcto sería eliminarla usando el comando:

```console
git branch -d bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitBranch3.png"/>
</div>

Para ver el historial con las ramas ahora veríamos:

```console
git log --graph --all --oneline
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog7.png"/>
</div>

---

## Ejercicio 9

En este ejercicio tendremos que:

~~~
- Crear la rama bibliografía.
- Cambiar a la rama bibliografía.
- Cambiar el fichero bibliografia.txt para que contenga las siguientes referencias:

Scott Chacon and Ben Straub. Pro Git. Apress. Ryan Hodson. Ry’s Git Tutorial. Smashwords (2014)

- Cambiar a la rama main.
- Cambiar el fichero bibliografia.txt para que - contenga las siguientes referencias:

Chacon, S. and Straub, B. Pro Git. Apress.

Loeliger, J. and McCullough, M. Version control with Git. O’Reilly.

- Añadir los cambios a la zona de intercambio temporal y hacer un commit con el mensaje "Añadida nueva referencia bibliográfica."
- Fusionar la rama bibliografía con la rama main.
- Resolver el conflicto dejando el fichero bibliografia.txt con las referencias:

Chacon, S. and Straub, B. Pro Git. Apress.

Loeliger, J. and McCullough, M. Version control with Git. O’Reilly.

Hodson, R. Ry’s Git Tutorial. Smashwords (2014)

- Añadir los cambios a la zona de intercambio temporal y hacer un commit con el mensaje "Resuelto conflicto de bibliografía."
- Mostrar la historia del repositorio incluyendo todas las ramas.
~~~

Para este ejercicio volveremos a hacer uso de la rama bibliografía por lo que volveremos a crear una rama para hacer este trabajo usando el comando:

```console
git branch bibliografia
git checkout bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitBranch4.png"/>
</div>

Ahora modificaremos el fichero bibliografia.txt usando:

```console
nano bibliografia.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia2.png"/>
</div>

Para que ponga:

*Scott Chacon and Ben Straub. Pro Git. Apress.*

*Ryan Hodson. Ry's Git Tutorial. Smashwords (2014)*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia3.png"/>
</div>

Lo siguiente que haremos será guardar los cambios hechos en la rama commiteando:

```console
git add .
git commit -m "Añadida nueva referencia bibliográfica"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit7.png"/>
</div>

Ahora tocará modificar la rama main por lo que tendremos que acceder a ella:

```console
git checkout main
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCheckout2.png"/>
</div>

Ahora modificaremos en la rama main el mismo archivo que en la rama bibliografía creando un conflicto.

```console
nano bibliografia.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia4.png"/>
</div>

*Chacon, S. and Straub, B. Pro Git. Apress.*

*Loeliger, J. and McCullough, M. Version control with Git. O'Reilly.*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia5.png"/>
</div>

Ahora que ya lo hemos hecho guardaremos los cambios en esta rama usando:

```console
git add .
git commit -m "Añadida nueva referencia bibliográfica"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit8.png"/>
</div>

Ahora intentaremos juntar las ramas viéndose el conflicto que tendremos que solucionar:

```console
git merge bibliografia
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitMerge2.png"/>
</div>

Como dice el mensaje tendremos que resolver el conflicto por lo que haremos:

```console
nano bibliografia.txt
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia6.png"/>
</div>

Y aquí modificaremos el archivo para que contenga:

*Chacon, S. and Straub, B. Pro Git. Apress.*

*Loeliger, J. and McCullough, M. Version control with Git. O’Reilly.*

*Hodson, R. Ry’s Git Tutorial. Smashwords (2014)*

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/NanoBibliografia7.png"/>
</div>

Ahora que el conflicto ya está resuelto queda subir los cambios con:

```console
git add .
git commit -m "Solucionado conflicto bibliografia"
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitCommit9.png"/>
</div>

Lo último que nos queda hacer es comprobar cómo nos ha quedado el historial con las ramas usando:

```console
git log --graph --all --oneline
```

<div align="center">
    <img src="../Imágenes/Manipulación de repositorios avanzada/GitLog8.png"/>
</div