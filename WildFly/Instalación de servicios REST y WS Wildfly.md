# Tarea: Instalación de servicios REST/WS Wildfly

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Portada.png"/>
</div>

## Índice

- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20servicios%20REST%20y%20WS%20Wildfly.md#requisitos)
- [Servicio RS](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20servicios%20REST%20y%20WS%20Wildfly.md#servicio-rs)
- [Servicio WS](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20servicios%20REST%20y%20WS%20Wildfly.md#servicio-ws)

---

## Requisitos

En esta ocasión tendremos que ayudarnos de dos archivos de ejemplos situados en este repositorio:

- [https://github.com/wildfly/quickstart.git](https://github.com/wildfly/quickstart.git)

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Repositorio.png"/>
</div>

Para disponer de los archivos que se encuentran en este repositorio tendremos que clonarlo, ya bien usando "git clone" en la consola o ayudándonos de un IDE.

En el caso de usar git clone sería de esta manera:

```console
git clone https://github.com/wildfly/quickstart.git 
```

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/GitClone.png"/>
</div>

Aparte de esto tendremos que contar con los softwares de MAVEN, Java y por último tener [Wildfly](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Instalaci%C3%B3n%20de%20WildFly.md) instalado.

---

## Servicio RS

Para la instalación de este servicio lo primero que tendremos que hacer es dirigirnos a la consola del sistema y acceder a la carpeta donde se encuentra el software que hemos clonado, en específico el de rest, que es con el que estamos tratado ahora mismo. Para ello usaremos el comando:

```console
cd Escritorio/quickstart/helloworld-rs
```

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/EntrarRs.png"/>
</div>

Una vez dentro de este archivo lo siguiente será hacer uso de MAVEN para compilar el proyecto usando:

```console
mvn clean install
```

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Compilar1.png"/>
</div>

Donde tras finalizar nos saldrá algo así:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/ResulCompilacion1.png"/>
</div>

Ahora debemos acceder a nuestro servicio de Wildfly y dirigirnos a la pestaña de Deploymets donde tendremos que buscar la opción de la siguiente imagen y marcarla:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Deployments.png"/>
</div>

Le saldrá una ventana donde tendrá que poner la dirección de la ruta del archivo war del servicio que queremos desplegar. En nuestro caso esa ruta es:

- Escritorio/quickstart/helloworld-rs/target/helloworld-rs.war

Tras terminar de rellenar la ventana nos saldrá en Wildfly algo similar a:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/DeplymentRsCompleto.png"/>
</div>

A su vez podríamos acceder al servicio desde el navegador usando el puerto de Wildfly y el servicio de esta manera:

- localhost:8084/helloworld-rs

En este caso nos mostraría:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/HelloWoldRs.png"/>
</div>

---

## Servicio WS

Para la instalación de este servicio lo primero que tendremos que hacer es dirigirnos a la consola del sistema y acceder a la carpeta donde se encuentra el software. Para ello usaremos el comando:

```console
cd Escritorio/quickstart/helloworld-ws
```

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/EntrarWs.png"/>
</div>

Una vez dentro de este archivo lo siguiente será hacer uso de MAVEN para compilar el proyecto usando:

```console
mvn clean install
```

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Compilar2.png"/>
</div>

Donde tras finalizar nos saldrá algo así:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/ResulCompilacion2.png"/>
</div>

Ahora debemos acceder a nuestro servicio de Wildfly y dirigirnos a la pestaña de Deploymets donde tendremos que buscar la opción de la siguiente imagen y marcarla:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/Deployments.png"/>
</div>

Le saldrá una ventana donde tendrá que poner la dirección de la ruta del archivo war del servicio que queremos desplegar. En nuestro caso esa ruta es:

- Escritorio/quickstart/helloworld-ws/target/helloworld-ws.war

Tras terminar de rellenar la ventana nos saldrá en Wildfly algo similar a:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/DeplymentWsCompleto.png"/>
</div>

A su vez podríamos acceder al servicio desde el navegador usando el puerto de Wildfly y el servicio de esta manera:

- localhost:8084/helloworld-ws

En este caso nos mostraría:

<div align="center">
    <img src="../Imágenes/Instalación de servicios REST y WS Wildfly/HelloWoldWs.png"/>
</div>