# Instalación de PHP

<div align="center">
    <img src="../Imágenes/Instalación de PHP/Portada.png"/>
</div>

## Índice

- [Actualizar paquetes](https://github.com/RubenGonz/Despliegues/blob/main/PHP/Instalacion%20de%20PHP.md#actualizar-paquetes)
- [PHP para Apache](https://github.com/RubenGonz/Despliegues/blob/main/PHP/Instalacion%20de%20PHP.md#php-para-apache)
- [PHP para Nginx](https://github.com/RubenGonz/Despliegues/blob/main/PHP/Instalacion%20de%20PHP.md#php-para-nginx)
- [Probar PHP](https://github.com/RubenGonz/Despliegues/blob/main/PHP/Instalacion%20de%20PHP.md#probar-php)

---

## Actualizar paquetes

Lo primero que debemos hacer por normal general a la hora de hacer cualquier instalación en el sistema de Ubuntu para cubrirse las espalda es actualizar el sistema de paquetes que nos aporta el propio sistema operativo. Para ello debemos ejecutar el comando:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/ActualizarPaquetes.png"/>
</div>

---

## PHP para Apache

Ahora que ya tenemos actualizados nuestros paquetes lo siguiente que debemos hacer es usar el paquete libapache2-mod-php. Sin embargo el único comando que necesitamos es:

```console
sudo apt install -y php
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/InstalarPHP1.png"/>
</div>

Y ya lo tendríamos instalado.

---

## PHP para Nginx

Por otra parte si lo queremos usar en Nginx tenemos que usar php-fpm. La manera de instalarlo así es:

```console
sudo apt install -y php-fpm
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/InstalarPHP2.png"/>
</div>

Ahora que lo hemos instalado nos hace falta configurar Nginx para que reconozca php-fpm. Para ello entramos en:

```console
sudo nano /etc/nginx/sites-available/default
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/EntrarConfiguracion.png"/>
</div>

Dentro de este archivo tendremos que buscar las líneas de código de la imagen y descomentarlas dejando el archivo similar a:

<div align="center">
    <img src="../Imágenes/Instalación de PHP/ModificarConfiguración.png"/>
</div>

Ahora que ya lo hemos modificado tendriamos que recargar el servicio de Nginx con:

```console
sudo systemctl reload nginx
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/RecargarNginx.png"/>
</div>

---

## Probar PHP

Hay muchas maneras de probar si php está en nuestro equipo, pero dos maneras sencillas podrían ser usando php -v y php info.

La primera manera sería usando php -v que es un comando que tenemos disponible gracias a que tenemos php-fpm que consta con la opción de trabajar en consola. Por ello si nos sale la versión de php que estamos usando debe estar instalada.

```console
php -v
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/ComprobarVersion.png"/>
</div>

La otra manera es usando un archivo dentro de /var/www/html/ que conste de php. Un archivo sencillo es uno que tenga phpinfo(). Para ello haremos:

```console
sudo nano /var/www/html/info.php
```

<div align="center">
    <img src="../Imágenes/Instalación de PHP/CrearInfo.png"/>
</div>

Y dentro de este archivo escribiremos:

~~~
<?php phpinfo();
~~~

<div align="center">
    <img src="../Imágenes/Instalación de PHP/InfoPhp.png"/>
</div>

Por lo que al acceder a la página del localHost y a info.php veríamos:

<div align="center">
    <img src="../Imágenes/Instalación de PHP/VistaFinal.png"/>
</div>

Y sabríamos que tenemos PHP en nuestros equipos.