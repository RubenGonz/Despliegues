# Instalación de Apache Tomcat

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/Portada.png"/>
</div>

## Índice

- [Requisitos de la instalación](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache%20Tomcat.md#requisitos-de-la-instalaci%C3%B3n)
- [Instalación](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache%20Tomcat.md#instalaci%C3%B3n)
- [Acceso](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache%20Tomcat.md#acceso)

---

## Requisitos de la instalación

Para la instalación de Apache-Tomcat hará falta de una cuenta de superusuario en un Ubuntu 20.04, por ejemplo y además disponer de por lo menos un puerto para soportar Apache.

---

## Instalación

Lo primero que debemos hacer para realizar la instalación de manera segura será actualizar los repositorios que nos administra el sistema operativo de Ubuntu. Para ello haremos:

```console
sudo apt update && sudo apt upgrade
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/ActualizarPaquetes.png"/>
</div>

Una vez actualizado los paquetes lo siguiente será descargarnos desde el sitio web oficial el paquete que queramos usar, nosotros lo haremos através de la consola usando la herramienta wget de esta manera:

```console
wget https://downloads.apache.org/tomcat/tomcat-10/v10.0.12/bin/apache-tomcat-10.0.12.tar.gz
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/Descarga.png"/>
</div>

Ahora que ya lo tenemos lo siguiente que debemos hacer es crear un usuario nuevo para que tomcat trabaje con él por lo que usaremos el comando:

```console
sudo useradd -U -m -d /opt/tomcat -k /dev/null -s /bin/false tomcat
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/CrearUser.png"/>
</div>

A continuación descomprimimos el paquete descargado usando:

```console
sudo tar xf apache-tomcat-10.0.12.tar.gz -C /opt/tomcat/
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/Descomprimir.png"/>
</div>

Ahora especificaremos que el dueño del archivo es el usuario tomcat que hemos creado usando:

```console
sudo chown -R tomcat: /opt/tomcat/
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/AsignarUser.png"/>
</div>

Para mayor comodidad crearemos un enlace simbólico para poder acceder a los archivos de forma más sencilla usando:

```console
sudo ln -s /opt/tomcat/apache-tomcat-10.0.12/ /opt/tomcat/apache-tomcat
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/EnlaceSimbolico.png"/>
</div>

Lo siguiente que haremos será configurar este servidor por lo que accederemos y editaremos su configuración:

```console
sudo nano /etc/systemd/system/tomcat10.service
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/EditarConfiguracion.png"/>
</div>

Aquí escribiremos:

~~~~
[Unit]

Description=Tomcat 10.0 servlet container para Ubuntu 20.04 LTS

After=network.target

[Service]

Type=forking

User=tomcat

Group=tomcat

Environment="JAVA\_OPTS=-Djava.security.egd=file:///dev/urandom" Environment="CATALINA\_BASE=/opt/tomcat/apache-tomcat" Environment="CATALINA\_HOME=/opt/tomcat/apache-tomcat" Environment="CATALINA\_PID=/opt/tomcat/apache-tomcat/temp/tomcat.pid" Environment="CATALINA\_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC" ExecStart=/opt/tomcat/apache-tomcat/bin/startup.sh ExecStop=/opt/tomcat/apache-tomcat/bin/shutdown.sh

[Install]

WantedBy=multi-user.target
~~~~

Quedando de esta manera:

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/Configuracion.png"/>
</div>

Ahora ya podemos iniciar el servidor que lo haremos usando:

```console
sudo systemctl start tomcat10
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/ArrancarTomcat.png"/>
</div>

Podría fallar por no tener el puerto por defecto(8080) disponible por lo que haremos será modificar el archivo server.xml con:

```console
sudo nano /opt/tomcat/apache-tomcat/conf/server.xml
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/EditarPuertos.png"/>
</div>

Cambiando su contenido haciendo que el puerto sea el 8083,por ejemplo.

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/Puertos.png"/>
</div>

Para especificar que queremos que al arrancar el servicio cada vez encendamos el pc escribiremos:

```console
sudo systemctl enable tomcat10
```

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/ArrancarInicio.png"/>
</div>

---

## Acceso

Accederemos usando nuestro navegador y dirigiéndonos al puerto por defecto de la máquina (8080) o al dirigido a nuestra preferencia como en nuestro caso que fue al puerto 8083.

<div align="center">
    <img src="../Imágenes/Instalación de Apache Tomcat/AccesoWeb.png"/>
</div>

Con esto ya podríamos trabajar con Apache-Tomcat.
