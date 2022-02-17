# Servicios RESTful con Tomcat y Jersey

<div align="center">
    <img src="../Imágenes/Servicios RESTful con Tomcat y Jersey/Portada.png"/>
</div>

---

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Servicios%20RESTful%20con%20Tomcat%20y%20Jersey.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Servicios%20RESTful%20con%20Tomcat%20y%20Jersey.md#requisitos)
- [Construcción del proyecto](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Servicios%20RESTful%20con%20Tomcat%20y%20Jersey.md#construcci%C3%B3n-del-proyecto)
- [Errores](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Servicios%20RESTful%20con%20Tomcat%20y%20Jersey.md#errores)

---

## Introducción

En esta ocasión lo que vamos a implantar será un servicio RESTful ayudándonos de dos herramientas como son Jersey y Tomcat 10. Aparte de estas también usaremos Maven, la cuál gestionará nuestras dependencias del proyecto.

En el caso de una Api Rest como la que crearemos se aprovechará de los métodos HTTP que són:

- Get: Busca datos.
- Put: Altera los datos.
- Post: Crea nuevos datos.
- Dlete: Elimina datos.

---

## Requisitos

Para realizar esta práctica tendremos que haber realizado con antelación el despliegue de una aplicación en Tomcat tal y como se hace en:

- [Despliegue de un War en Apache-Tomcat](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md)

Y a parte la instalación de un cliente Rest como puede ser:

- [Advanced Rest Client](https://install.advancedrestclient.com/install)

Ahora que ya tenemos los conocimientos necesarios empecemos:

---

## Construcción del proyecto

En esta ocasión lo que haremos será hacer uso de un proyecto con el servicio Rest ya incorporado como el siguiente:

- [Aplicación con servicio Rest](https://github.com/jpexposito/docencia/tree/master/COMUN/ejemplos/java/rest-service)

Y construirlo haciendo uso de:

```console
mvn clean install
```

Para que nos genere el war y :

```console
sudo cp Escritorio/rest-service/target/rest-service.war webapps/webapps
```

Para que nos lo muestre en nuestro tomcat.

Ahora para acceder a nuestra aplicación iriamos a nuestro manager como en otras ocasiones saltandonos una ventana como la siguiente:

<div align="center">
    <img src="../Imágenes/Servicios RESTful con Tomcat y Jersey/Error.png"/>
</div>

## Errores

Como ya hemos visto no obtenemos la respuesta esperada. Para ver porqué es así deberemos dirigirnos a:

```
/opt/tomcat/apache-tomcat/logs/catalina."Fecha".log
```
```
/opt/tomcat/apache-tomcat/logs/localhost."Fecha".log
```

En estos dos archivos deberíamos poder ver salidas como por ejemplo:

<div align="center">
    <img src="../Imágenes/Servicios RESTful con Tomcat y Jersey/SalidaLog.png"/>
</div>

En mi caso no encontré un error que pudiese solucionar a través de la incorporación de una nueva dependencia, cambio en el código de la aplicación o configuración de Tomcat. Pidiendo ayuda a mis compañeros he visto que algunos obtienen uno o varios mensajes de error que en mi caso no se encuentran por lo que no puedo seguir avanzando con la práctica.
