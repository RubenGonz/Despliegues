# Instalación y administración de servidores de transferencia de archivos en local

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/Portada.png"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#introducci%C3%B3n)
- [Instalación de FTP](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#instalaci%C3%B3n-de-ftp)
- [Comprobación del servicio](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#comprobaci%C3%B3n-del-servicio)
- [Configurar el servidor FTP](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#configurar-el-servidor-ftp)
- [Modo Pasivo](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#modo-pasivo)
- [Verificación de acceso](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#verificaci%C3%B3n-de-acceso)
- [Seguridad TLS](https://github.com/RubenGonz/Despliegues/blob/main/FTP/Instalaci%C3%B3n%20y%20administraci%C3%B3n%20de%20servidores%20de%20transferencia%20de%20archivos.md#seguridad-tls)

---

## Introducción

FTP es un protocolo de red usado para transferir archivos entre sistemas conectados ambos a TCP. Este protocolo es muy usado sobre todo en pequeñas y medianas empresas.

---

## Requisitos

Para la instalación y configuración sólo tendremos que contar con Internet y un sistema Ubuntu 20.04.

---

## Instalación de FTP

Para la instalación de FTP lo primero que haremos será actuallizar los paquetes que nos proporciona el sistema operativo de Linux. Para ello ejecutaremos el comando:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/ActualizarPaquetes.png"/>
</div>

Y ahora que ya tenemos los paquetes actualizados lo siguiente será instalarlo usando:

```console
sudo apt install -y vsftpd
```

Donde veremos:

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/Instalación.png"/>
</div>

Ahora que ya lo tenemos en nuestro sistema lo siguiente que haremos será permitir el acceso a los puertos estandar de FTP y el puerto de datos del mismo protocolo. Solo necesitaremos hacer:

```console
sudo ufw allow ftp
sudo ufw allow ftp-data
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/PermitirAcceso.png"/>
</div>

---

## Comprobación del servicio

Para comprobar que hemos hecho la instalación correctamente podemos comrpobar el estado de una de las herramientas instaladas donde solo tendriamos que poner:

```console
systemctl status vsftpd
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/ComprobarServicio.png"/>
</div>

---

## Configurar el servidor FTP

El archivo de configuración principal de FTP lo encontraremos dentro de la carpeta de /etc/ ubicada en el archivo *vsftpd.conf*.

Para poder acceder y configurar este protocolo debemos contar con un cliente, por lo que en el caso de no tenerlo lo instalaremos con:

```console
sudo apt install filezilla
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/InstalaciónFirezilla.png"/>
</div>

Ahora que ya lo tenemos haremos unas cuantas configuraciones:

---

### Modo pasivo

La configuración por defecto de nuestro servidor FTP estará en modo Activo, lo que implica que solo permite conexiones entre  clientes que no se encuentren tras un firewall y que no intentan cambiar al modo pasivo.

Para cambiar esto lo que haremos en primer lugar será editar *vsftpd.conf*:

```console
sudo nano /etc/vsftpd.conf
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/editarConf.png"/>
</div>

Añadiendo al final del fichero:

```console
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=30050
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/ContenidoConf.png"/>
</div>

Donde estamos indicando el rango de puertos disponibles que mas nos interese.

Para guardar los cambios y que tengan un efecto real deberemos recargar el servicio usando:

```console
sudo systemctl reload vsftpd
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/recargarServicio.png"/>
</div>

Y posteriormente permitir acceso al rango de puertos que decidimos anteriormente.

```console
sudo ufw allow 30000:30050/tcp
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/permitirPuertos.png"/>
</div>

Ahora cuando queramos usar el modo pasivo lo unico que tendremos que hacer es usar el siguiente comando despues de la autenticación: 

```console
passive
```

---

### Verificación de acceso

El servidor tiene dos formas con las que trabajar que son usando un acceso anónimo o un usuario del sistema. Por defecto se usa la opción de usuarios del sistema que necesitan autenticarse previamente. 

En el caso de que la quieras úniamente para usuarios anónimos buscaremos la opción "local_enable" en nuestro archivo de configuración y la cambiaremos a *NO* como en el ejemplo:

```console
local_enable=NO
```

Sin embargo si todavía quieres mantener el acceso a los usuarios locales puedes permitirles cosa como crear o eliminar archivos y directorios descomentando la siguiente línea:

```console
#write_enable=YES
```

Quedando el archivo:

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/ContenidoAcceso.png"/>
</div>

Otra característica de la configuración bastante reseñable es restringir a los usuarios para que solo puedan acceder a sus directorios personales debido a que por defecto un usuario normal puede acceder a toda la red de directorios.

Para hacer esto lo que debemos hacer es buscar y descomentar en nuestra configuración:

```console
#chroot_local_user=YES
```

Y posteriormente añadimos:

```console
allow_writeable_chroot=YES
```

Debido a que sino podría haber problemas con directorios esenciales del sistema.

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/AccesoUsuarios.png"/>
</div>

---

### Seguridad TLS


Para acceder con usuarios externos es necesario usar conexiones cifradas por lo que se utiliza una clave RSA y un certificado, que ya nos facilita Ubuntu.

Sin embargo, este certificado normalmente está creado para el nombre de máquina, por lo que podría mostrar una advertencia que puede ser molesta y poco profesional.

Como alternativa podemos instalar certificados firmados para el dominio con el que vayamos a conectar, o crear certificados autofirmados.

Por ejemplo en el caso de que quisieramos crear un certidicado para el dominio *daw.dpl*.

Una manera de hacerlos sería usando el comando openssl:

```console
sudo openssl req -x509 -newkey rsa:2048 -days 3650 -nodes -out /etc/ssl/certs/daw.dpl.crt -keyout /etc/ssl/private/daw.dpl.key
```

Donde tendremos que cumplimentar los datos que nos pidan prestandole atención al de Common Name:

```console
Common Name (e.g. server FQDN or YOUR name) []:daw.dpl
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/Certificado.png"/>
</div>

Ahora lo que haremos será volver a modificar nuestro fichero de configuración donde buscaremos:

```console
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
```

Y lo sustituiremos por:

```console
rsa_cert_file=/etc/ssl/certs/daw.dpl.crt
rsa_private_key_file=/etc/ssl/private/daw.dpl.key
ssl_enable=YES
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/certificadoConf.png"/>
</div>

Con esto habremos cambiado las rutas del certificado y la clave privada.

Para guardar nuestros avances en la configuración y ver la respuesta de nuestra nueva configuración lo que haremos será recargar el servicio con:

```console
sudo systemctl restart vsftpd
```

<div align="center">
    <img src="../Imágenes/Instalación y administración de servidores de transferencia de archivos en local/ReiniciarServicio.png"/>
</div>