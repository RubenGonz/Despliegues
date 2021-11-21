# Ejercicio práctico

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/Portada.png"/>
</div>

---

En este ejercicio crearemos un repositorio llamado "ejercicio_git_nombre_apellido1_apellido2", por lo que crearemos una carpeta con este nombre y dentro de ella, inicializaremos un repositorio usando:

```console
mkdir ejercicio_git_nombre_apellido1_apellido2
cd ejercicio_git_nombre_apellido1_apellido2/
git init
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/InicializarRepositorio.png"/>
</div>

Lo siguiente que tendremos que hacer es crear el archivo README.md y commitearlo por lo que haremos:

```console
touch README.md
git add .
git commit -m "Creación del archivo README.md"
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/CommitReadMe.png"/>
</div>

Lo siguiente que nos pide es crear una rama llamada feature-1 y moverse a ella.

```console
git branch feature-1
git checkout feature-1
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/GitBranch1.png"/>
</div>

Ahora aquí crearemos el archivo hola.html y le pondremos un texto usando:

```console
touch hola.html
nano hola.html
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/CrearHola.png"/>
</div>

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/NanoHola.png"/>
</div>

Con esto hecho guardaremos los datos con:

```console
git add .
git commit -m "Creación del archivo hola.html"
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/CommitHola.png"/>
</div>

Lo siguiente será movernos a la rama principal y crear el archivo adios.html usando:

```console
git checkout master
touch adios.html
nano adios.html
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/CrearAdios.png"/>
</div>

Con el texto especificado:

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/NanoAdios.png"/>
</div>

Guardaremos:

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/CommitAdios.png"/>
</div>

Y lo uniremos todo en una rama usando el comando:

```console
git merge feature-1
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/GitMerge.png"/>
</div>

Los cambios se verían como:

```console
git log
```

<div align="center">
    <img src="../Imágenes/Ejercicio práctico/GitLog.png"/>
</div>

Con esto ya estaría el ejercicios terminado.