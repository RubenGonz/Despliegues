# Despliegue de un War en Apache-Tomcat

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/Portada.png"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md#introducci%C3%B3n)
- [Carpetas importantes](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md#carpetas-importantes)
- [Terminología](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md#terminolog%C3%ADa)
- [Creación de una App Web en Java](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md#creaci%C3%B3n-de-una-app-web-en-java)
- [Despliegue](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Despliegue%20de%20un%20War%20en%20Apache-Tomcat.md#despliegue)

---

## Introducción

Apache Tomcat como uno de los servidores más usados y populares entre la gran variedad de servidores que hay en el mercado, ha conseguido entre otras muchas cosas que sus usuarios puedan usar servicios o aplicaciones empaquetadas como aplicaciones web de una manera muy sencilla y en esta página verá cómo hacerlo.

---

## Carpetas importantes

-  $CATALINA_HOME

Esta variable será el punto de instalación de nuestro Tomcat.

- $CATALINA_BASE  

Esta variable será el punto de despliegue de nuestro Tomcat, que suele estar asociada al valor de $CATALINA_HOME. Por ejemplo, una aplicación la podríamos encontrar en $CATALINA_HOME\webapps.

---

## Terminología

- Root. Es la carpeta raíz o padre de nuestra aplicación.
- Context path. Es la carpeta que representa el nombre de la aplicación web. Por ejemplo si nuestra aplicación se llamase 'miAplicacion' nuestra aplicación se desplegará en $CATALINA_HOME\webapps\miAplicacion, el acceso desde el navegador lo veremos en http://localhost/miAplicacion y el contexto estaría en /miAplicacion.
- War. Es la extensión de nuestros ficheros de la aplicación Web. Para la creación de los ficheros con extensión _war_ podemos hacer usos de herramientas como Maven que nos ayuda con la construcción de este tipo de ficheros y su despliegue.

Para añadir nuestro tomcat a un proyecto le incrustaríamos este texto a nuestro pom.xml:

```
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.2</version>
  <configuration>
    <url>http://localhost:8082/manager/text</url>
    <server>TomcatServer</server>
    <path>/myapp</path>
  </configuration>
</plugin>
```

Para realizar el despliegue tendremos el comando:

```console
mvn tomcat7:deploy
```

En el caso de que queramos redesplegar una aplicación también disponemos de:

```console
mvn tomcat7:redeploy
```

Y para terminar el despliegue tenemos:

```console
mvn tomcat7:undeploy
```

---

## Creación de una App Web en Java

Lo siguiente que haremos será hacer uso de un proyecto que ya habremos creado u obtenido. En mi caso usaré el que se encuentra en:

- [Proyecto de Java](https://github.com/jpexposito/docencia/tree/master/COMUN/ejemplos/java/app-web-demo)

Una vez lo tenemos modificaremos el fichero web.xml cambiando:

```
<display-name>app-web-alumno</display-name>  
```

por:

```
<display-name>app-web-ruben</display-name>
```
Entre otros cambios...

Para construir nuestro war de la aplicación tendremos que hacer:

```console
mvn clean install
```

Que si todo va bien nos deberá salir un mensaje tal como:

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/mvnClean.png"/>
</div>

Esto nos creará una carpeta target con 'app-web-ruben.war'. 

---

## Despliegue

Para desplegar nuestro war creamos una carpeta nueva dentro de nuestro root con:

```console
cd ~
mkdir webapps && cd webapps
```

Y hacerle un enlace simbólico a /opt/tomcat/apache-tomcat/webapps/ con:

```console
sudo ln -s /opt/tomcat/apache-tomcat/webapps/ .
```

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/enlaceSimbolico.png"/>
</div>

Ahora usaremos nuestro archivo war copiandolo a la carpeta webapps/webapps con:

```console
sudo cp Escritorio/Proyecto-java/target/app-web-ruben.war webapps/webapps
```

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/copiarWar.png"/>
</div>

Posteriormente accederemos añ manager de nuestro tomcar através de:

- http://localhost:8083/manager/html

Donde veremos:

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/manager.png"/>
</div>

Nos saldrán nuestros distintos war y su estado como en nuestro caso que es activo.

Accediendo a http://localhost:8083/app-web-ruben/ veremos nuestra aplcación desplegada:

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/VistaFinal.png"/>
</div>