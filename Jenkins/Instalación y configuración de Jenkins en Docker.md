# Instalación y configuración de Jenkins en Docker

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

---

## Introducción

En esta ocasión lo que haremos será instalar la herramienta Jenkins en Docker dentro de nuestro sistema Ubuntu 20.04, alojando dentro de un dominio local que habremos creado en apache.

---

## Requisitos

Para esta instalación debemos contar anteriormente con Apache en nuestro sistema, Docker y Docker-compose en un sistema Ubuntu 20.04.

En el caso de no contar con Apache puedes hacer uso de:

- [Instalación de Apache En Ubuntu](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache2.md)

En el caso de no contar con la instalación de Docker podemos acceder a:

- [Instalación de Apache En Ubuntu](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache2.md)

Por último si no contamos con el docker-compose tendremos que actualizar nuesrto sistema de paquetes con:

```console
sudo apt update
sudo apt upgrade
```

Y una vez terminado 

 para instalarlo solo tendremos que actualizar el sistema de paquetes que nos proporciona linux:

```console
sudo apt update
```

Po