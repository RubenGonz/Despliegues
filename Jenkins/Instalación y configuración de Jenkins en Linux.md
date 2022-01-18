# Instalación y configuración de Jenkins en Linux

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Creación del dominio local]()
- [Instalación de Jenkins]()
- [Comprobacion final]()

---

## Introducción

En esta ocación lo que haremos será instalar la herramienta Jenkins en nuestro sistema Ubuntu 20.04, alojandola dentro de un dominio local que habremos creado en apache.

---

## Requisitos 

Para esta instalación debemos contar anteriormente con Apache en nuestro sistema, Java y un sistema Ubuntu 20.04.

En el caso de no contar con Apache puedes hacer uso de:

- [Instalación de Apache En Ubuntu](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache2.md)

En el caso de no contar con la instalación de Java para instalarlo solo tendremos que actualizar el sistema de paquetes que nos proporciona linux:

```console
sudo apt upate
```

Posteiormente tendremos que buscar los paquetes openjdk con:

```console
sudo apt search openjdk
```

Elegir la opción que queramos, en mi caso:

```console
sudo apt install openjdk-11-jdk
```

Una vez que termine el proceso comprobar que esta instalado viendo la salida del comando:

```console
java -version
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/VersionJava.png"/>
</div>

---

## Creación del dominio local

Para la creación de nuestro dominio local en apache lo primero que tendremos que hacer es crear la carpeta donde alojaremos nuestro dominio local, en nuestro caso será:

```console
sudo mkdir -p /var/www/rubengric.com/public_html
```

Despues cambiaremos la propiedad de este archivo al user www-data.

```console
sudo chown -R www-data: /var/www/rubengric.com
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/PropietarioCarpeta.png"/>
</div>

Ahora lo que haremos será cambiar el contenido del archivo de configuración.

```console
sudo nano /etc/apache2/sites-available/rubengric.com.conf
```

Donde pondremos nuestra configuración:

```
<VirtualHost *:80>
    ServerName rubengric.com
    ServerAlias www.rubengric.com
    ServerAdmin ruben30303030@gmail.com
    DocumentRoot /var/www/rubengric.com/public_html
 
    <Directory /var/www/rubengric.com/public_html>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>
 
    ErrorLog ${APACHE_LOG_DIR}/rubengric.com-error.log
    CustomLog ${APACHE_LOG_DIR}/rubengric.com-access.log combined

    RedirectMatch ^/$ http://rubengric.com:8080
</VirtualHost>
```

Lo siguiente será habilitar nuestro dominio con:

```console
sudo a2ensite rubengric.com
```

Donde nos pedirá que recarguemos el apache con:

```console
systemctl reload apache2
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/RecargarApache.png"/>
</div>

Y hacer un enlace simbólico con:

```console
sudo ln -s /etc/apache2/sites-available/rubengric.com.conf /etc/apache2/sites-enabled/
```

Ahora reiniciaremos nuestro Apache con:

```console
sudo systemctl restart apache2
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/ReiniciarApache.png"/>
</div>

Ahora nos quedaría agregar nuestro dominio a la carpeta:

```console
sudo nano /etc/hosts
```

Añadiendo:

```
127.0.0.1       rubengric.com www.rubengric.com
```

Y conprobamos que se despliega acuediendo a nuestro navegador y poniendo:

```console
http://rubengric.com
```

## Instalación de Jenkins

Para la instalación de Jenkins nos podemos ayudar del sistema de paquetes que nos proporciona Ubuntu. Por ello procederemos a actualizar y mejorar nuestros paquetes del sistema con:

```console
sudo apt update
sudo apt upgrade
```

Ahora ya podriamos usar el comando:

```console
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/InstalacionJenkins1.png"/>
</div>

```console
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/InstalacionJenkins2.png"/>
</div>

```console
sudo apt-get update
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/InstalacionJenkins3.png"/>
</div>

```console
sudo apt-get install jenkins
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/InstalacionJenkins4.png"/>
</div>

Para comprobar que lo hemos instalado correctamente podriamos hacer:

```console
sudo systemctl daemon-reload
sudo systemctl status jenkins
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/ComprobacionJenkins.png"/>
</div>

Para ver la interfaz gráfica podemos acceder al puerto 8080 a traveés del navegador con:

```console
http://localhost:8080
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/VistaJenkins.png"/>
</div>

Donde nos indicara que pongamos nuestra contraseña, esa se encontrará en la ruta de la imagen, es decir dentro de:

```console
sudo nano /var/lib/jenkins/secrets/initialAdminPassword
```

Donde al acceder veremos nuestra contraseña, en mi caso:

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/ContraseniaJenkins.png"/>
</div>

Copiaremos el contenido y lo pegaremos en el input que nos deja Jenkins en la pantalla del navegador.

Ahora ya podriamos acceder a nuestro Jenkins, eligiendo que plugins instalar y posteriormente accediendo a la creacion del usuario Admin:

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/LogJenkins.png"/>
</div>

Donde tras rellenar los datos nos mostrará una pantalla similar a la siguiente:

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/RutaJenkins.png"/>
</div>

Esta pagina nos pedirá establecer la URL donde funcionará nuestro Jenkins. En nuestro caso le deberemos poner la de nuestro dominio:

```console
http://rubengric.com
```

---

## Comprobación final 

Para comprobar que todo funcionacorrectamente lo que debería pasar es que al acceder a nuestro dominio, es decir a:

```console
http://rubengric.com
```

Nos debería dirigir directamente a:

```console
http://rubengric.com:8080
```

Que será donde nos encontraremos con nuestro Jenkins.

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/PantallaFinal.png"/>
</div>

Donde tras registrarnos no mostrará:

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Linux/JenkinsFinal.png"/>
</div>

Y con ello tendriamos acceso a la herramienta de Jenkins con nuestro dominio.