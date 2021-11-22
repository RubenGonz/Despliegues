# Instalación de Nginx

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/Portada.png"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#requisitos)
- [Instalación](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#instalaci%C3%B3n)
- [Configuración firewall](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#configuraci%C3%B3n-firewall)
- [Comprobar estado del servidor](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#comprobar-estado-del-servidor)
- [Administrar proceso](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#administrar-proceso)
- [Configurar bloques de servidor](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#configurar-bloques-de-servidor)
- [Archivos y directorios importantes](https://github.com/RubenGonz/Despliegues/blob/main/Instalaci%C3%B3n%20de%20Nginx/Instalaci%C3%B3n%20de%20Nginx.md#archivos-y-directorios-importantes)

---

## Introducción

Nginx es uno de los servidores web más famosos que puede albergar gran tráfico de datos en internet. Tiene la característica de ser muy ligero y poder administrar más de un dominio en el servidor de manera sencilla.

---

## Requisitos

Un sistema operativo Ubuntu 20.04 y un usuario non-root con privilegios sudo.

---

## Instalación

Para la instalación de este servidor lo primero que haremos, qué es lo que se debería hacer cada vez que procedemos con una instalación, es actualizar los paquetes que nos aporta el sistema operativo. Para actualizar el sistema tendremos que hacer:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ActualizarPaquetes.png"/>
</div>

Ahora que ya tenemos actualizado los paquetes lo siguiente será instalarlo ayudándonos de apt de esta manera:

```console
sudo apt install nginx
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/InstalarGinx.png"/>
</div>

Para comprobar que lo hemos instalado correctamente podemos comprobar el estado del servicio usando:

```console
sudo systemctl status nginx
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/GitStatus1.png"/>
</div>

---

## Configuración firewall

Ahora lo que haremos será ajustar el sistema para permitirle el acceso a este servicio. Lo primero que haremos será listar las aplicaciones con las que ufw puede trabajar:

```console
sudo ufw app list
```
La respuesta espera debe contener:

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ListaUfw.png"/>
</div>

Cada uno de estos perfiles tiene una serie de restricciones. El más restrictivo y el que usaremos será Nginx HTTP. Para usar este perfil haremos:

```console
sudo ufw allow 'Nginx HTTP'
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/PermitirNgix.png"/>
</div>

Para comprobar que lo hemos hecho correctamente accederemos a los perfiles que deja acceso el firewall, donde deben estar los nuestros:

```console
sudo ufw status
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/GitStatus2.png"/>
</div>

---

## Comprobar estado del servidor

Para comprobar que tenemos el servicio activo lo más sencillo es comprobarlo usando:

```console
sudo ufw status
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/GitStatus2.png"/>
</div>

---

## Administrar proceso

Para la administración del proceso tenemos varios comandos que nos pueden ayudar, los primeros tres son:

```console
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

Estos tres comandos son los encargados de gestionar si quieres que el servicio funcione, se pare y se reinicie, en ese mismo orden.

Por otro lado tenemos opciones para gestionar el arranque del servicio. Tenemos:

```console
sudo systemctl reload nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

En la primera opción tenemos la opción de recargar el servicio, es decir que vuelva a cargar sin perder las conexiones, la segunda opción hace que cuando el servidor arranque automáticamente también se inicie el servicio y por último tenemos la tercera opción que en el caso de que la 2 opción esté habilitada, la desactiva.

---

## Configurar bloques de servidor

Ahora lo siguiente que haremos será hacer la configuración para poder manejar más de un dominio en el propio sistema. Lo normal sería alojar nuestro dominio dentro de /var/www/html pero lo que haremos nosotros para cumplir con lo dicho será crear un sistema de directorios dentro de /var/www que albergará nuestros dominios, dejando /var/www/html para otros casos.

Lo primero que haremos será crear la carpeta que maneja nuestro dominio usando -p para crear algunos directorios necesarios:

```console
sudo mkdir -p /var/www/el_dominio/html
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/CrearDominio.png"/>
</div>

Lo siguiente será asignarle la propiedad del directorio, en este caso ayudándonos de la variable $USER:

```console
sudo chown -R $USER:$USER /var/www/el_dominio/html
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/AsignarPropietario.png"/>
</div>

Para asegurarnos de que el directorio tiene los permisos correctos se los asignaremos a mano para asegurarnos. Esto lo haremos usando chmod de esta manera:

```console
sudo chmod -R 755 /var/www/el_dominio
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/AsignarPermisos.png"/>
</div>

Vamos a crear la página principal de nuestro dominio que se llamará index.html y dentro ponerle algo de código.

```console
nano /var/www/el_dominio/html/index.html
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/CrearIndex.png"/>
</div>

Este será el código que le pondremos:

~~~
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>

    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
</html>
~~~

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/InsertarCodigo.png"/>
</div>

Lo siguiente que haremos será crear un archivo de configuración para manejar el dominio. Este será:

```console
sudo nano /etc/nginx/sites-available/el_dominio
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/CrearConfiguracion.png"/>
</div>

En este archivo pondremos la información sobre el puerto que tendrá el dominio, su propio nombre, entre otras cosas:

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ContenidoConf.png"/>
</div>

Ahora que ya lo hemos configurado lo siguiente será crear un enlace simbólico entre este archivo y el directorio donde Nginx tiene las páginas habilitadas.

```console
sudo ln -s /etc/nginx/sites-available/el_dominio /etc/nginx/sites-enabled/
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/CrearEnlaceSimbolico.png"/>
</div>

Al crear este nuevo bloque dedicado a nuestro dominio podemos aprovechar mejor los directorios debido a que de manera normal las solicitudes que se dirijan a nuestro dominio accederán al bloque creado específicamente para el dominio y las solicitudes que no se dirijan aquí se dirigirán al bloque original que las gestionará por su cuenta.

Otra cosa que tenemos que hacer es modificar el archivo hosts para añadir nuestro dominio, esto lo haremos con:

```console
sudo nano /etc/hosts/
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/EditarConfGeneral.png"/>
</div>

Dentro de aquí añadiremos nuestro dominio de esta manera:


<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ModificarConfGeneral.png"/>
</div>

Ahora que ya tenemos esto vamos a acceder a la configuración de Nginx para evitar posibles problemas de memoria.

```console
sudo nano /etc/nginx/nginx.conf
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/CongiguracionNginx.png"/>
</div>

Dentro de este archivo buscaremos la opción de la imagen, la descomentamos quitando el símbolo # y la dejaremos a 64:


<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ConfNginxModificada.png"/>
</div>

Ahora que ya hemos terminado de editar la configuración del servicio Nginx vamos a asegurarnos de no tener errores en la sintaxis de los archivos mirando la respuesta de:

```console
sudo nginx -t
```

La respuesta debe ser:

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ComprobarSintaxis.png"/>
</div>

Por último lo que debemos hacer es reiniciar Nginx para habilitar todos los cambios.

```console
sudo systemctl restart nginx
```

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/ReiniciarNginx.png"/>
</div>

Una vez hecho esto para comprobar el dominio podríamos ir a nuestro navegador preferido e introducir en el buscador nuestro dominio con el puerto de esta manera:

- [http://el_dominio:8085/](http://el_dominio:8085/)

Donde la salida sería el html introducido anteriormente:

<div align="center">
    <img src="../Imágenes/Instalación de Nginx/SalidaFinal.png"/>
</div>

Ahora ya tendríamos nuestro dominio montado.

---

## Archivos y directorios importantes

~~~
/var/www/html

Aquí se encuentra el contenido de las páginas web, que en el caso de que no tengamos ninguna configurada nos saldrá una página por defecto.
~~~

~~~
/etc/nginx

Este es el directorio donde se encuentra toda la configuración del servicio Nginx.
~~~

~~~
/etc/nginx/nginx.conf

Este es el archivo principal de configuración del funcionamiento general del servicio en sí.
~~~

~~~
/etc/nginx/sites-available

En este directorio tendremos todos los bloques de servidor, aunque realmente para que funcionen tienen que estar vinculados a /etc/nginx/sites-enabled.
~~~

~~~
/etc/nginx/sites-enabled

Aquí tendremos todos los bloques que han sido habilitados de /etc/nginx/sites-available.
~~~

~~~
/etc/nginx/snippets

Aquí tendremos fragmentos de configuración que tenemos separados y que podemos usar en diversas partes para manejar el servicio de manera más sencilla.
~~~

~~~
/var/log/nginx/access.log

Este es el archivo donde le llegan todas las solicitudes de los servidores web.
~~~

~~~
/var/log/nginx/error.log

Este es el archivo de Nginx donde se guardan todos los errores ocurridos en el servicio.
~~~