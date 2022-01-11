# Instalación y administración de servidores de transferencia de archivos en local

<div align="center">
    <img src="../Imágenes/Construcción de un servicio de una empresa/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

---

## Introducción

En esta ocación lo que haremos será crear un servicio que va a contar con:

- Un dominio principal y un subdominio
- Un servidor de base de datos
- La herramienta PhpMyAdmin
- El soporte de Php
- Un servidor de SFTP

Todo englobado dentro de un contenedor Docker.

Tendremos un dominio principal que será desde donde accederemos a nuestro servicio, dentro de este podremos acceder a nuestro PhpMyAdmin o a nuestro subdominio.

Dentro de PhpMyAdmin tendremos que poder hacer uso de nuestra base de datos y dentro de nuestro subdominio debemos tener acceso a los archivos que hayamos subido a través de SFTP.

---

## Requisitos

Para la instalación y configuración sólo tendremos que contar con Internet y un sistema Ubuntu 20.04.

---

## instalacion


sudo nano /etc/hosts