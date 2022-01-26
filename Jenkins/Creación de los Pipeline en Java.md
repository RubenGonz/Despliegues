## Creación de los Pipeline en Java

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Creación del repositorio]()
- [Configuración en Jenkins]()

---

## Introducción

En esta ocasión lo que haremos será usar un pipeline para comprobar el despliegue de un apache-tomcat que cuente con java que habremos creado previamente en un repositorio de GitHub. Este contará con una parte donde construiremos nuestra aplicación, una parte donde obtendremos la respuesta de si se ha desplegado o no correctamente, la comprobación de que nos muestra el texto esperado y otra donde podremos eliminar o no, depende de nuestro gusto si queremos eliminar el contenedor de la aplicación tras su testeo.

---

## Requisitos

Para la realización de esta práctica será necesario contar con Jenkins instalado en nuestro sistema. En el caso de no contar con él tienes a tu disposición este enlace que te ayudará con la instalación.

- [Instalación y configuración de Jenkins en Linux](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Linux.md)

También sería adecuado tener un manejo básico de como crear un pipeline en Jenkins, en su defecto puedes contar con este informe que te ayudará:

- [Fundamentos de un Pipeline Jenkins](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Fundamentos%20de%20un%20Pipeline%20Jenkins.md)

---

# Creación del repositorio

Esta vez vamos a usar un proyecto que tendremos disponible en:

- [Enlace del proyecto en GitHub](https://github.com/RubenGonz/Proyecto-java.git)

Este contendrá la siguente estructura de ficheros:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/EstructuraFicheros.png"/>
</div>

Donde tendremos una __carpeta src__ que será la que contendrá nuestra aplicación en java, un __dockerfile__ que nos servirá para desplegar un tomcat, nuestro __jenkinsfile__ que testeará el despliegue de nuestra aplicación y el __pom.xml__ que tendrá la configuración, depedencias y plugins de neustra app.

La carpeta src será la que contendrá nuestro __index.jsp__ que contendrá el home de nuestra página:

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
   <body>
      <h1>Página principal de Ruben Gonzalez Rodriguez</h1>
      <p>Este es el puerto: ${pageContext.request.serverPort}</p>
   </body>
</html>
```

Y por otra parte nuestro __web.xml__ que tendrá:

```
<?xml version="1.0" encoding="UTF-8"?>
 <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
   xmlns="http://java.sun.com/xml/ns/javaee" 
     xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
       http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" 
         id="WebApp_ID" version="3.0">
 
     <display-name>app-web-ruben</display-name>
     <welcome-file-list>
         <welcome-file>index.jsp</welcome-file>
     </welcome-file-list>
 </web-app>
```

Nuestro __Dockerfile__, encargado de desplegar nuestra aplicación en tomcat tendrá:

```
FROM tomcat:9.0
LABEL maintainer="ruben303030@gmail.com"

ARG WAR_FILE=target/*.war

ADD ${WAR_FILE} /usr/local/tomcat/webapps/

EXPOSE 80

CMD ["catalina.sh", "run"]
```

La pipeline contenida en nuestro __Jenkinsfile__ tendrá una parte comentada debido a que nuestro desarrollo esta hecho para que la aplicación se llega a desplegar. En el caso de que unicamente se quiera comprobar su despliegue sin llegar a desplegar la aplicación podemos quitar la parte comentada:

```
pipeline {
    agent any
    stages {
        stage('Tests de la aplicacion') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Construccion') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Despliegue') {
            steps {
                sh 'docker build -t proyecto-java .'
                sh 'docker run -d --rm -p 8082:8080 --name ContenedorProyectoJava proyecto-java'
            }
        }
        stage('Test del despliegue') {
            steps {
                sh 'wget -q localhost:8082/app-web-ruben -O - | grep Ruben'
            }
        }
//
//      En el caso de querer borrar el contenedor tras testearlo pondremos:
//
//        stage('Borar contenedor') {  
//            steps {
//                sh 'docker stop ContenedorProyectoJava'  
//            }
//        }
    }
}
```

Por último en el __pom.xml__ tendremos:

```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>es.system.alumno</groupId>
  <artifactId>app-web-ruben</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>app-web-ruben</name>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>15</maven.compiler.source>
    <maven.compiler.target>15</maven.compiler.target>
    <jetty.port>8082</jetty.port>
    <maven-war-plugin-version>3.3.1</maven-war-plugin-version>
    <jetty-maven-plugin-version>11.0.7</jetty-maven-plugin-version>
    <maven-compiler-plugin-version>3.8.0</maven-compiler-plugin-version>
  </properties>

  <dependencies>
  
  
  </dependencies>

  <build>
    <finalName>app-web-ruben</finalName>
    <plugins>
    <plugin>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>${maven-compiler-plugin-version}</version>
      <configuration>
        <source>${maven.compiler.source}</source>
        <target>${maven.compiler.target}</target>
      </configuration>
    </plugin>
    <plugin>
      <artifactId>maven-war-plugin</artifactId>
      <version>${maven-war-plugin-version}</version>
      <configuration>
        <failOnMissingWebXml>false</failOnMissingWebXml>
      </configuration>
    </plugin>
    <plugin>
      <groupId>org.eclipse.jetty</groupId>
      <artifactId>jetty-maven-plugin</artifactId>
      <version>${jetty-maven-plugin-version}</version>
      <configuration>
        <webApp>
        </webApp>
        <httpConnector>
          <port>${jetty.port}</port>
        </httpConnector>
      </configuration>
    </plugin>
  </plugins>
  </build>
</project>
```

Ahora ya tendriamos nuestro proyecto en GitHub y podriamos crear nuestra pipeline.

---

## Configuración en Jenkins

Ahora que ya tenemos nuestro proyecto en el repositorio nos dirigiremos a nuestro Jenkins donde iremos a crearnos un nuevo pipeline que se ejecutará desde nuestro GitHub, lo construiriamos y deberíamos tener una imagen similar a:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/SalidaTest.png"/>
</div>

Y en el caso de no haber puesto en el Jenkinsfile la opción de borrar el contenedor tras el test tendríamos una salida como la siguiente en la web donde veríamos que está corriendo:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/SalidaWeb.png"/>
</div>