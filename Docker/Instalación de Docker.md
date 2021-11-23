# Instalación de Docker

<div align="center">
    <img src="../Imágenes/Instalación de Docker/Portada.png"/>
</div>

## Índice

- [Instalación]()
- [Imagen de docker]()
- [Administrar contenedores]()

---

## Instalación

Para ejecutar la instalación de Docker podemos proceder de dos maneras, usando los repositorios que nos proporciona Ubuntu o por otro lado recurrir a la página oficial de Docker. En el caso de esta instalación vamos a usar la segunda opción debido a que así nos aseguraremos tener la versión más reciente en el momento que hagamos la instalación.

Lo primero que debemos hacer es actualizar el sistema de paquetes para cubrirnos las espaldas con la instalación. Para ello haremos:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/ActualizarPaquetes.png"/>
</div>

Lo siguiente que haremos será instalar algunos paquetes de requisitos. Solo tendremos que ejecutar:

```console
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/InstalarRequisitos.png"/>
</div>

Ahora que ya tenemos los requisitos usaremos una clave GPG para el repositorio oficial de Docker.

```console
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/AccederAlRepositorio.png"/>
</div>

Por último antes de la instalación agregaremos el repositorio de Docker a APT usando:

```console
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/AgregarRepositorio.png"/>
</div>

Ahora que ya hemos añadido docker a APT lo siguiente que haremos será volver a actualizar los paquetes que nos proporciona Ubuntu usando nuevamente:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/ActualizarPaquetes.png"/>
</div>

Para asegurarnos de instalar la versión establecida por nosotros y no la que nos dispone Ubuntu usaremos el comando:

```console
apt-cache policy docker-ce
```

En el caso de que lo hayamos hecho correctamente nos saldrá algo similar a:

<div align="center">
    <img src="../Imágenes/Instalación de Docker/ComprobarVersion.png"/>
</div>

Ahora si, podremos instalar docker fácilmente usando:

```console
sudo apt install docker-ce
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/InstalarDocker.png"/>
</div>

Para comprobar que lo hemos hecho correctamente podemos usar:

```console
sudo systemctl status docker
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/ComprobarEstado.png"/>
</div>

En el caso de que nos salga activo significa que lo hemos todo correctamente.

---

## Imagen de docker

Docker trabaja con imágenes, estas imágenes las podemos conseguir desde distintos medios como Docker Hub. Para hacer una prueba vamos a probar con:

```console
sudo docker run hello-world
```

En el caso de tenerlo todo bien el resultado esperado es:

<div align="center">
    <img src="../Imágenes/Instalación de Docker/HelloWorld.png"/>
</div>

---

## Administrar contenedores

Con el paso del tiempo usaremos gran cantidad de contenedores hasta el punto de que nos sea difícil administrarlos. Una manera cómoda para poder ver únicamente los contenedores que tengamos activos es:

```console
sudo docker ps
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/docker-ps.png"/>
</div>

Por otra parte para poder ver los contenedores que estén activos y los inactivos usaremos:

```console
sudo docker ps -a
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/docker-ps-a.png"/>
</div>

En el caso de que estemos trabajando con varios contenedores al mismo tiempo y quiera abrir el más reciente, es decir, el último que creó, usaremos:

```console
sudo docker ps -l
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/docker-ps-l.png"/>
</div>

Por último en el caso de que queramos listar las imágenes qie tengamos en Docker tenemos:

```console
sudo docker images
```

<div align="center">
    <img src="../Imágenes/Instalación de Docker/dockerImages.png"/>
</div>

Con esto ya podrías tener el control de Docker.
