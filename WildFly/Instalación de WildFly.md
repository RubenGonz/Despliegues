# Instalación de WildFly

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/Portada.png"/>
</div>

## Índice

- [¿Qué es?](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#qu%C3%A9-es)
- [Requisitos previos](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#requisitos-previos)
- [Actualización de repositorios](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#actualizaci%C3%B3n-de-repositorios)
- [Instalación](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#instalaci%C3%B3n)
- [Configuración](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#configuraci%C3%B3n)
- [Acceso](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#acceso)
- [Nuevos usuarios](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#nuevos-usuarios)
- [Gestión de consola](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md#gesti%C3%B3n-de-consola)

---

## ¿Qué es?

WildFly es una versión gratuita de JBoss que sirve como servidor de aplicaciones implementado totalmente usando Java y por lo tanto pudiéndose incorporar en cualquier sistema operativo.

---

## Requisitos previos

Para poder contar con este software debemos contar con otros con anterioridad.

Lo primero para el caso de esta instalación será contar con un sistema Ubuntu 20.04, además de ello deberfemos contar con las funcionalidades de superusuario. Dentro de este sistema operativo debemos tener instalado Java y por último tener instalado MAVEN.

---

## Actualización de repositorios

El sistema de paquetes que nos proporciona sistemas operativos como puede ser ubuntu o linux mint puede ser muy útil para la instalación de softwares o en su defecto tenerlo de forma segura por lo que antes de proceder con la instalación de WildFly actualizaremos los paquetes del sistema usando el comando:

```console
sudo apt update
```

Aquí te pedirá la contraseña de administrador donde tras colocarla empezaría el proceso y donde veremos algo similar a lo siguiente:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ActualizarPaquetes.png"/>
</div>

---

## Instalación

Lo primero para la instalación es conocer la versión que finalmente instalaremos y para ellos nos dirigiremos a:

- [https://www.wildfly.org/](https://www.wildfly.org/)

Donde buscaremos la versión que necesitemos. Después de esto nos dispondremos a descargarla usando la terminal, al acceder a ella simplemente tendremos que usar "wget" de esta manera:

```console
wget https://github.com/wildfly/wildfly/releases/download/25.0.0.Final/wildfly-25.0.0.Final.tar.gz
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/DescargarWildFly.png"/>
</div>

Con esto ya tendríamos descargada la versión elegida.

Lo siguiente será crear un grupo y usuario para que el servidor pueda trabajar con comodidad. Para crearlos simplemente tendremos que usar los comandos del sistema, en este caso serían:

```console
sudo groupadd -r wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearGrupo.png"/>
</div>

Para crear un grupo y:

```console
sudo useradd -r -g wildfly -d /opt/wildfly -s /sbin/nologin wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUser.png"/>
</div>

Para crear un usuario en ese grupo.

Ahora que ya tenemos esto hecho lo siguiente será descomprimir la versión descargada y moverla a donde corresponda usando:

```console
tar -xvzf wildfly-25.0.0.Final.tar.gz
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/Descomprimir.png"/>
</div>

```console
sudo mv wildfly-25.0.0.Final /opt/wildfly-25.0.0.Final
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/Mover.png"/>
</div>

Para mayor comodidad crearemos un enlace simbólico de esta manera:

```console
sudo ln -s /opt/wildfly-25.0.0.Final /opt/wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EnlaceSimbolico.png"/>
</div>

Ahora que ya lo tenemos descargado lo siguiente será darle acceso al usuario y al grupo creados para WildFly. Esto lo haremos con el mismo comando para las dos acciones:

```console
sudo chown -R wildfly:wildfly /opt/wildfly
sudo chown -R wildfly:wildfly /opt/wildfly/
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/AsignarPermisos.png"/>
</div>

Ahora crearemos una carpeta para wildfly y posteriormente copiaremos la configuración de la carpeta de /opt en esta carpeta creada.

```console
sudo mkdir -p /etc/wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearCaprpeta.png"/>
</div>

```console
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.conf /etc/wildfly/
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CopiarConfiguración.png"/>
</div>

Para comprobar que lo hemos hecho correctamente accederemos a la configuración:

```console
sudo nano /etc/wildfly/wildfly.conf
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EntrarConfiguracion.png"/>
</div>

Donde tendremos que tener algo similar a:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/Configuracion.png"/>
</div>

Lo siguiente que haremos será configurar el arranque para ellos usaremos estos comandos:

```console
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/launch.sh /opt/wildfly/bin/
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CopiarArranque.png"/>
</div>

Que copiara el arranque de /opt

```console
sudo sh -c 'chmod +x /opt/wildfly/bin/\*.sh'
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CopiarServicio.png"/>
</div>

Que copiara el servicio de /opt

```console
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/AsignarPermisos2.png"/>
</div>

Que asignará permisos

```console
sudo systemctl daemon-reload
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/RecargarDemonio1.png"/>
</div>

Y que recargará el demonio.

Lo siguiente será iniciar el servidor arrancando el servicio usando:

```console
sudo systemctl start wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ArrancarServicio.png"/>
</div>

Para comprobar que lo hemos hecho correctamente usaremos:

```console
sudo systemctl status wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EstadoServicio1.png"/>
</div>

En el caso de que quisiéramos que wildfly se arrancase cuando nuestro pc se inicie tendremos que usar:

```console
sudo systemctl enable wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ArrancarInicio.png"/>
</div>

---

## Configuración

Ahora que ya tenemos instalado el servicio tendremos que configurarlo. En este caso tendremos que cambiar el puerto que usará wildfly debido a que por defecto se usa el localhost y ya tenemos un servicio alojado.

Para ellos accederemos al archivo de configuración en:

```console
sudo nano /opt/wildfly/standalone/configuration/standalone.xml
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EntrarConfPuertos.png"/>
</div>

Y cambiaremos el puerto donde se indica en la imagen poniendo en que nosotros deseemos, en nuestro caso el 8084:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ConfPuertos.png"/>
</div>

Por último solo nos faltaría permitir el tráfico con:

```console
sudo ufw allow 8084/tcp
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/PermitirPuerto.png"/>
</div>

---

## Acceso

Para acceder a WildFly únicamente necesitaremos un navegador. Dentro del url del navegador tendremos que escribir nuestro localhost pero usando el puerto adecuado. La salida en pantalla sería similar a:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/LocalHost.png"/>
</div>

---

## Nuevos usuarios

Vamos a crearnos el usuario administrador de WildFly. Por ello ejecutaremos el script:

```console
sudo /opt/wildfly/bin/add-user.sh
```

Y elegiremos la opción "a", que es la de administrador.

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUsuarioAdmin1.png"/>
</div>

Ahora tendremos que establecer el nombre del usuario administrador y a posteriori la contraseña.

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUsuarioAdmin2.png"/>
</div>

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUsuarioAdmin3.png"/>
</div>

Por último nos pedirá que le indiquemos el grupo al que pertenece y no se lo especificaremos pulsando "Enter".

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUsuarioAdmin4.png"/>
</div>

Solo faltaría aceptar las demás condiciones y habriamos terminado de crear el usuario.

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/CrearUsuarioAdmin5.png"/>
</div>

---

## Gestión de consola

Para realizar la configuración lo haremos accediendo a:

```console
sudo nano /etc/wildfly/wildfly.conf
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EntrarConf.png"/>
</div>

Donde WILDFLY_CONSOLE_BIND será quien restrinja el acceso.

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EditarConf.png"/>
</div>

Y después accediendo a:

```console
sudo nano /opt/wildfly/bin/launch.sh
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EntrarArranque.png"/>
</div>

Donde comprobaremos que nos encontramos algo como esto:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ConfArranque.png"/>
</div>

Ahora deberemos configurar la manera en la que se lanza el servidor, por ello reiniciamos el servicio con:

```console
sudo systemctl restart wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/Reiniciar.png"/>
</div>

Después modificamos el archivo wildfly.service

```console
sudo nano /etc/systemd/system/wildfly.service
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EntrarServicio.png"/>
</div>

Cambiando dentro del fichero las líneas para que aparezca:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/EditarArranque.png"/>
</div>

Lo siguiente será recargar el demonio que corre por detrás y reiniciar el servicio.

```console
sudo systemctl daemon-reload
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/RecargarDemonio.png"/>
</div>

```console
sudo systemctl restart wildfly
```

<div align="center">
    <img src="../Imágenes/Instalación de WildFly/ReiniciarWildfly.png"/>
</div>

Con esto ya estaría terminada la configuración de WildFly y podríamos empezar a usarlo en nuestros sistemas.
