# Instalación y configuración de Jenkins en Docker

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Instalación del dominio local]()
- [Instalación de Jenkins con Docker]()
- [Instalación de Jenkins con Docker-compose]()

---

## Introducción

En esta ocasión lo que haremos será instalar la herramienta Jenkins en Docker dentro de nuestro sistema Ubuntu 20.04, alojando dentro de un dominio local que habremos creado en apache.

---

## Requisitos

Para esta instalación debemos contar anteriormente con Apache en nuestro sistema, Docker y Docker-compose en un sistema Ubuntu 20.04.

En el caso de no contar con Apache puedes hacer uso de:

- [Instalación de Apache En Ubuntu](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache2.md)

En el caso de no contar con la instalación de Docker podemos acceder a:

- [Instalación de Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md)

Por último si no contamos con el docker-compose tendremos que actualizar nuesrto sistema de paquetes con:

```console
sudo apt update
sudo apt upgrade
```

Y una vez terminado de actualizar tendriamos que ejecutar el comando:

```console
sudo apt install docker-compose
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/DockerComposeVersion.png"/>
</div>

---

## Creación del dominio local

Para la creación de nuestro dominio local en apache lo primero que tendremos que hacer es crear la carpeta donde alojaremos nuestro dominio local, en nuestro caso será:

```console
sudo mkdir -p /var/www/jenkins.rubengr.com/public_html
```

Después cambiaremos la propiedad de este archivo al user www-data.

```console
sudo chown -R www-data: /var/www/jenkins.rubengr.com
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/PropietarioCarpeta.png"/>
</div>

Ahora lo que haremos será cambiar el contenido del archivo de configuración.

```console
sudo nano /etc/apache2/sites-available/jenkins.rubengr.com.conf
```

Donde pondremos nuestra configuración:

```
<VirtualHost *:80>
    ServerName jenkins.rubengr.com
    ServerAlias www.jenkins.rubengr.com
    ServerAdmin ruben30303030@gmail.com
    DocumentRoot /var/www/jenkins.rubengr.com/public_html
 
    <Directory /var/www/jenkins.rubengr.com/public_html>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>
 
    ErrorLog ${APACHE_LOG_DIR}/jenkins.rubengr.com-error.log
    CustomLog ${APACHE_LOG_DIR}/jenkins.rubengr.com-access.log combined

    RedirectMatch ^/$ http://jenkins.rubengr.com:8080
</VirtualHost>
```

Lo siguiente será habilitar nuestro dominio con:

```console
sudo a2ensite jenkins.rubengr.com
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
sudo ln -s /etc/apache2/sites-available/jenkins.rubengr.com.conf /etc/apache2/sites-enabled/
```

Ahora reiniciaremos nuestro Apache con:

```console
sudo systemctl restart apache2
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/ReiniciarApache.png"/>
</div>

Ahora nos quedaría agregar nuestro dominio a la carpeta:

```console
sudo nano /etc/hosts
```

Añadiendo:

```
127.0.0.1       jenkins.rubengr.com www.jenkins.rubengr.com
```

Y comprobamos que se despliega acudiendo a nuestro navegador y poniendo:

```console
http://jenkins.rubengr.com
```

---

## Instalación de Jenkins con Docker

Lo primero que haremos será descargarnos la imagen que querremos usar de Jenkins, para ello iremos a:

```
https://hub.docker.com/
```

Una ves la hallamos encontrado pondremos docker pull y el nombre de nuestra imagen en mi caso:

```console
sudo docker pull jenkins/jenkins:lts
```

Para comprobar que tenemos la imagen podemos usar el comando:

```console
sudo docker images
```

Donde nos debería salir algo similar a:

```
REPOSITORY        TAG       IMAGE ID       CREATED      SIZE
jenkins/jenkins   lts       9aee0d53624f   7 days ago   441MB
```

Ahora que ya tenemos nuestra imagen lo siguiente que haremos será usar un contenedor con esta imagen de docker en el puerto 8080. Para ello con anterioridad en mi caso creare una carpeta que será donde voy a establecer la ruta home de nuestro Jenkins. 

```console
mkdir JenkinsHome
```

Ahora si, podemos arrancar el contenedor con:

```console
sudo docker run -p 8080:8080 -p 50000:50000 -v /home/rubengonz/JenkinsHome:/var/jenkins_home jenkins/jenkins:lts
```

Donde para comprobar el funcionaniemto podriamos ir a:

```
http://localhost:8080
```

Que es donde tenemos alojado el contenedor o en su lugar a nuestro dominio que nos redirige a nuestro jenkins:

```
http://jenkins.rubengr.com/
```

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/VistaFinalDocker.png"/>
</div>

---

## Instalación de Jenkins con Docker-compose

La otra manera alternativa a instalar en local y usar una imagen de docker en usar el docker compose y crear nuestra propia imagen y lanzar el contenedor a nuestro gusto.

Lo primero que haremos será cear nuestro sistema de ficheros para el uso del dockercompose. Este tendrá la siguiente apariencia:

<div align="center">
    <img src="../Imágenes/Instalación y configuración de Jenkins en Docker/SistemaFicheros.png"/>
</div>

Para ello dentro del fichero DockerFile pondremos la información de nuestra nueva imagen.

```
FROM jenkins/jenkins

USER root
RUN apt-get -y update && apt-get install -y maven

USER jenkins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
```

En el fichero Plugins.txt tendremos todos los plugins que nos pondremos dentro de nuestra imagen Jenkins, en nuestro caso:

```
ace-editor
ant
antisamy-markup-formatter
apache-httpcomponents-client-4-api
authentication-tokens
branch-api
build-monitor-plugin
build-pipeline-plugin
cloudbees-folder
conditional-buildstep
copyartifact  
credentials
credentials-binding
deploy
display-url-api
docker-commons
docker-workflow
durable-task
git
github
github-api
git-client
git-server
gradle
greenballs
handlebars
jackson2-api
javadoc
jquery
jquery-detached
jsch
junit
mailer
matrix-project
maven-plugin
momentjs
nested-view
parameterized-trigger
pipeline-build-step
pipeline-graph-analysis
pipeline-input-step
pipeline-milestone-step
pipeline-model-api
pipeline-model-declarative-agent
pipeline-model-definition
pipeline-model-extensions
pipeline-rest-api
pipeline-stage-step
pipeline-stage-tags-metadata
pipeline-stage-view
plain-credentials
run-condition
scm-api
script-security  
ssh-credentials
structs
token-macro
workflow-aggregator
workflow-api
workflow-basic-steps
workflow-cps
workflow-cps-global-lib
workflow-durable-task-step
workflow-job
workflow-multibranch
workflow-scm-step
workflow-step-api
workflow-support
```

Y por último nuestro fichero docker-compose.yml que será el que nos ayudará a desplegar nuestro Jenkins.

```
version: '3'
services:
  master:
    build: jenkins
    image: imagenJenkins
    restart: unless-stopped
    hostname: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home

volumes:
  jenkins_home:
```

