# Instalación y administración de servidores de transferencia de archivos en docker

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/Portada.png">
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#requisitos)
- [Buscar imagenes](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#buscar-imagenes)
- [Usar la imagen](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#usar-la-imagen)
- [Comprobar la imagen](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#comprobar-la-imagen)
- [Configurar home de un usuario](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#configurar-home-de-un-usuario)
- [Configurar home de varios usuarios](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos%20en%20docker.md#configurar-home-de-varios-usuarios)

---

## Introducción

FTP es un protocolo de red usado para transferir archivos entre sistemas conectados ambos a TCP. Este protocolo es muy usado sobre todo en pequeñas y medianas empresas.

---

## Requisitos

Para la instalación y configuración sólo tendremos que contar con Internet, un sistema Ubuntu 20.04. y docker instalado en nuestro sistema.

---

## Buscar imagenes

Lo primero que haremos sera obtener la imagen de Docker para SFTP que decidamos. Para buscarlas podemos ver las que nos aporta: 

- https://hub.docker.com

Para obtener los resultados de buscar "*sftp*" usaremos el comando:

```console
sudo docker search sftp
```

Donde nos saldrá una lista de imagenes de "*sftp*" como la siguiente:

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/ListaImagenes.png">
</div>

De entre las opciones que nos muestra la consola podremos elegir la que más nos convenga.

---

## Usar la imagen

Una vez elegida nuestra imagen lo siguiente que haremos será arrancarla usando el comando:

```console
sudo docker run --name mysftp -p 2294:22 -d atmoz/sftp admin:admin:::upload
```

Donde:

- name: Es el nombre que le daremos al contenedor.
- p: Es el puerto que le asignaremos, primero del host y después del contenedor.
- d: Imagen que usaremos para crear el contenedor.
- admin: Donde upload es un archivo guardado en /home/foo/upload.

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/ArrancarImagen.png">
</div>

---

## Comprobar la imagen

Una vez que tengamos la imagen arrancada podremos comprobarlo haciendo:

```console
sudo docker ps | grep sftp
```

Donde tendremos que ver:

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/ComprobarImagen.png">
</div>

Ahora, para ver si podemos acceder a nuestro servidor FTP vamos a comprobar su dirección IP usando el identificador de la imagen obtenida en el anterior comando:

```console
sudo docker inspect a3485c0302a1 | grep "IPAddress"
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/ComprobarIp.png">
</div>

---

## Configurar home de un usuario

Para configurar un usuario o los usuarios ace falta configurar un directorio Home. Esto lo haremos con el comando:

```console
docker run --name mysftp2 -v /host/upload:/home/admin/upload --privileged=true -p 2295:22 -d atmoz/sftp admin:admin:1001
```

Donde:

- name: Tenemos que generar un nuevo nombre debido a que este no se puede repetir, al igual que el puerto.
- v: Aqui estableceremos el home donde indicaremos el directorio del host y el directorio en el contenedor. En el caso de que local/host/uplaod no exista se crea.
- privileged: Aqui indicaremos que le queremos añadir privilegios al contenedor debido a que es necesario por la seguridad de selinux.

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/ConfigurarHome.png">
</div>

---

## Configurar home de varios usuarios

Para configurar varios usuarios tenemos dos alternativas:

- Creemos usuarios en el contenedor y asignamos permisos.
- Escribimos archivos de usuario en el host y los montamos en el contenedor.

Para el primer metodo podemos ayudarnos del comando del ejemplo anterior.

Para el 2 metodo tendremos que cerar /host/users.conf en nuestro directorio local y ejecutar:

```console
sudo docker run --name mysftp3 -v /host/users.conf:/etc/sftp/users.conf:ro -v /home/sftp:/home --privileged=true -p 2296:22 -d atmoz/sftp
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/CrearContenedorUsuarios.png">
</div>

Donde indicaremos el archivo que contendrá nuestra configuración de los usuarios.

Después solo tendremos que editar /host/users.conf en el directorio local e indicar los usuarios siguiendo la estructura de:

```console
"NombreUser":"ContraseñaUser":"IdUser":"IdGrupo"
```

Como en el ejemplo:

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en docker/EstructuraUser.png">
</div>

Ahora ya tendriamos los usurios creados.
