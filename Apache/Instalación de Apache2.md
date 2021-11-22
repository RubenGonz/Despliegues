# Apache2

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/Portada.png"/>
</div>

## Índice

- [¿Qué es?]()
- [Requisitos de la instalación]()
- [Actualización de los paquetes]()
- [Instalación de Apache2]()
- [Configuración de los puertos]()
- [Acceso]()
- [Configurar firewall]()

---

## ¿Qué es?

Apache2 es un servidor web de código abierto muy extendido y soportado por muchas plataformas.

---

## Requisitos de la instalación

Para la instalación de Apache-Tomcat hará falta de una cuenta de superusuario en un Ubuntu 20.04, por ejemplo y además disponer de por lo menos un puerto para soportar Apache.

En nuestro caso hemos liberado el puerto 80, siendo este el que está por defecto para hacer una instalación más sencilla usando:

```console
sudo systemctl stop (nombre del servicio que ocupa el puerto 80)
```

Aunque si no hiciéramos este paso también podríamos completar la instalación.

---

## Actualización de los paquetes

La manera más sencilla de instalar Apache2 es a través de los repositorios que nos administra el sistema operativo de Ubuntu. Para ello actualizaremos los paquetes usando:

```console
sudo apt update && sudo apt upgrade
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ActualizarPaquetes.png"/>
</div>

---

## Instalación de Apache2

Lo primero que haremos una vez completados los pasos anteriores será instalarlo usando en la terminal:

```console
sudo apt install apache2
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/InstalarApache2.png"/>
</div>

Ya una vez instalado podremos ver en el navegador al acceder a nuestro localhost una imagen similar a la siguiente:

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/Localhost1.png"/>
</div>

---

## Configuración de los puertos

Ahora, que pasa si tuviésemos activos otro servicio corriendo ya en el localhost. Nos saldria un error similar a:

~~~
apache2.service: Control process exited, code=exited, status=1/FAILURE
systemd[1]: apache2.service: Failed with result 'exit-code'.
systemd[1]: Failed to start The Apache HTTP Server.
~~~

El motivo es que el puerto por defecto ya está ocupado. Para solucionar esto debemos acceder a:

```console
sudo nano /etc/apache2/ports.conf
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/EditarPuertos.png"/>
</div>

Y cambiar "Listen 80" por "Listen 8081".

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ConfiguracionPuerto1.png"/>
</div>

Despues tambien debemos acceder a:

```console
sudo nano /etc/apache2/sites-enabled/000-default.conf
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/EditarSitesDisponibles.png"/>
</div>

Y cambiar "<VirtualHost: \*:80>" por "<VirtualHost: \*:8081>".

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ConfiguracionPuerto2.png"/>
</div>

Ahora solo tendremos que reiniciar apache usando:

```console
sudo systemctl restart apache2
sudo service apache2 restart
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ReiniciarApache.png"/>
</div>

---

## Acceso

Ya tendríamos disponible Apache2 en el puerto 8081. Para acceder al navegador pondremos:

- (Nuestra ip):8081

O por otra parte:

- localhost:8081

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/Localhost2.png"/>
</div>

---

## Configurar firewall

Ahora que ya tenemos Apache2 instalado lo siguiente será configurar el firewall ya que por defecto tendremos UFW. Para comprobarlo deberíamos tener una salida similar a esta cuando escribimos:

```console
sudo ufw app list
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ConfiguracionEstandar.png"/>
</div>

En esta ocasión usaremos el perfil Apache. Por ello escribiremos:

```console
sudo ufw allow 'Apache'
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/PerfilApache.png"/>
</div>

Para comprobar que lo hemos hecho correctamente podremos usar:

```console
sudo ufw status
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ComprobarEstado1.png"/>
</div>

En el caso de que nos de que el servicio está inactivo tendremos que ejecutar el comando:

```console
sudo ufw enable
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ActivarServicio.png"/>
</div>

Ahora al hacer el comando sudo ufw status nos dará:

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ComprobarEstado2.png"/>
</div>

Para comprobar que todo se está ejecutando correctamente escribiremos:

```console
sudo systemctl status apache2
```

Cuyo resultado será similar a:

<div align="center">
    <img src="../Imágenes/Instalación de Apache2/ComprobarApache2.png"/>
</div>

Si nos sale una imagen similar a la mostrada significará que lo hemos hecho todo correcctamente asi que ya podrá hacer uso de Apache2.
