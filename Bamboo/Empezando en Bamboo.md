# Empezando en Bamboo

<div align="center">
    <img src="Imagenes/BombooLogo.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

---

## Introducción

En 

---

## Requisitos

- Java Home

## Instalación

- [Página de instalación](https://www.atlassian.com/software/bamboo/download)

- Get started

Elegir sistema operativo y empieza la descarga

Comprovar java home

```console
java -version
```

```console
echo $JAVA_HOME
```

```console
sudo /usr/sbin/useradd --create-home --home-dir /usr/local/bamboo --shell /bin/bash bamboo
```

Mover la carpeta descomprimida de la instalacion a una carpeta (Home)

Crear carpeta Home de bamboo en nuestro caso BambooHome

Acceder a ella y modificar el bamboo.home


nano atlassian-bamboo-8.1.2/atlassian-bamboo/WEB-INF/classes/bamboo-init.properties 

```
bamboo.home=/home/rubengonz/BambooHome/
```

Estado final:

```
## You can specify your bamboo.home and bamboo.shared.home property here or in >

#Local Bamboo home. If running in HA (multi-node) mode, you need to provide a u>

bamboo.home=/home/rubengonz/BambooHome/

#If running in HA mode, you need to provide a shared home directory that will b>
#It is allowed to mount this shared directory as a subdirectory of bamboo.home
#Can be left undefined when running in single node mode, in which case the data>

#bamboo.shared.home=
```

Ejecucion de nuestro bamboo

./bin/start-bamboo.sh

Nos saldra una salida como:

´´´
To run Bamboo in the foreground, start the server with start-bamboo.sh -fg

Server startup logs are located in /home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2/logs/catalina.out

Bamboo Data Center
   Version : 8.1.2
                  

If you encounter issues starting or stopping Bamboo Server, please see the Troubleshooting guide at https://confluence.atlassian.com/display/BAMBOO/Installing+and+upgrading+Bamboo

Using CATALINA_BASE:   /home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2
Using CATALINA_HOME:   /home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2
Using CATALINA_TMPDIR: /home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2/bin/bootstrap.jar:/home/rubengonz/Escritorio/InstalacionBamboo/atlassian-bamboo-8.1.2/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
´´´


Tienes que tener cuenta y licencia