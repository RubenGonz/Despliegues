# Despliegue de un War en Apache-Tomcat

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Carpetas importantes]()
- [Terminología]()
- [Creación de una App Web en Java]()

---

## Introducción

Apache Tomcat como uno de los servidores más usados y populares entre la gran variedad de servidores que hay en el mercado, ha conseguido entre otras muchas cosas que sus usuarios puedan usar servicios o aplicaciones empaquetadas como aplicaciones web de una manera muy sencilla y en esta página verá como hacerlo.

---

## Carpetas importantes

-  $CATALINA_HOME

Esta variable será el punto de instalación de nuestro Tomcat.

- $CATALINA_BASE  

Esta variable será el punto de despliegue de nuestro Tomcat, que suele estar asociada al valor de $CATALINA_HOME. Por ejemplo una aplicación la podríamos encontrar en $CATALINA_HOME\webapps.

---

## Terminología

- Root. Es la carpeta raíz o padre de nuestra aplicación.
- Context path. Es la carpeta que representa el nombre de la aplicación web. Por ejemplo si nuestra aplicación se llamase 'miAplicacion' nuestra aplicación se desplegaría en $CATALINA_HOME\webapps\miAplicacion, el acceso desde el navegador lo veremos en http://localhost/miAplicacion y el contexto estaría en /miAplicacion.
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

Esto nos creará una carpeta target con 'app-web-ruben.war'. Posteriormente si queremos deplegar el war con maven podemos eliminar nuestro pom.xml y ejecutar:

```console
mvn archetype:generate -DgroupId=es.iespuerto.ruben -DartifactId=app-ruben -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```  

Que nos creará una nueva estructura adecuada para la herramienta tal como la siguiente:

<div align="center">
  <img src="../Imágenes/Despliegue de un War en Apache-Tomcat/EstructuraMaven.png"/>
</div>
