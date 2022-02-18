# Clusterizando una App en Wildfly

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Construcción del proyecto]()
- [Despliegue con Jetty]()
- [Despliegue con Docker-compose]()

---

## Introducción

Esta vez lo que haremos será crear un cluster en Wildfly para una aplicación en Java. Tendremos alojado un total de 3 hosts diferentes haciendo uso de JBoss y docker-compose.

---

## Requisitos

Para esta nueva confguración necesitaremos:

- Sistema operativo Ubuntu
- Maven instalado
- Docker instalado
- Docker-compose instalado

## Construcción del proyecto

El proyecto que usaremos lo vamos a crear en Java o en su lugar obtenerlo desde el siguiente enlace:

- [Proyecto de Java](https://github.com/jpexposito/docencia/tree/master/COMUN/ejemplos/java/app-web-demo)

Ahora lo que haremos será modificar nuestro proyecto para que:

En el web.xml:

- Display-name:

```
<display-name>app-web-ruben</display-name>
```

En el index.jsp:

- Body:

```
<body>
    <h1>Página principal de Ruben Gonzalez Rodriguez</h1>
</body>
```

En el pom.xml:

- groupId:

```
<groupId>es.system.ruben</groupId>
```

- artifactId:

```
<artifactId>app-web-ruben</artifactId>
```

- name:

```
<name>app-web-ruben</name>
```

- url:

```
<url>https://www.ruben.com</url>
```

- properties:

```
<jetty.port>9000</jetty.port>
```

- build:

```
<finalName>app-web-ruben</finalName>
```

Y ahora ya podríamos construir nuestro proyecto con:

```console
mvn clean install
```

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/MvnCleanInstall.png"/>
</div>

---

## Despliegue con Jetty

Para proceder al despliegue con Jetty tendremos que tener los plugins adecuados en nuestro pom.xml como en el ptroyecto de ejemplo y una vez hecho así deberemos proceder a ejecutar nuestro Jetty con:

```console
mvn clean jetty:run
```

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/MvnCleanJetty.png"/>
</div>

Donde al acceder al puerto 9000, que es el que indicamos en nuestro pom.xml veremos:

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/Despliegue1.png"/>
</div>

## Despliegue con Docker-compose

Para los otros dos despliegues haremos uso de docker-compose y por ello lo primero que haremos será crear un Dockerfile para nuestro WildFly como el siguiente:

```
FROM jboss/wildfly

ARG WAR_FILE=target/*.war

ADD ${ARG} /opt/jboss/wildfly/standalone/deployments/

ARG WILDFLY_NAME
ARG CLUSTER_PW

ENV WILDFLY_NAME=${WILDFLY_NAME}
ENV CLUSTER_PW=${CLUSTER_PW}


ENTRYPOINT /opt/jboss/wildfly/bin/standalone.sh -b=0.0.0.0 -bmanagement=0.0.0.0 -Djboss.server.default.config=standalone-full-ha.xml -Djboss.node.name=${WILDFLY_NAME} -Djava.net.preferIPv4Stack=true -Djgroups.bind_addr=$(hostname -i) -Djboss.messaging.cluster.password=${CLUSTER_PW}
```

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/Dockerfile.png"/>
</div>

Y ahora si, nos crearemos el docker-compose.yml:

```
version: '3.5'
services:

  wildfly1:
    build:
      context: .
      args:
        WILDFLY_NAME: wildfly_1
        CLUSTER_PW: secret_password
    image: wildfly_1
    ports:
    - 9001:8080
    networks:
      wildfly_network:

  wildfly2:
    build: 
      context: .
      args:
        WILDFLY_NAME: wildfly_2
        CLUSTER_PW: secret_password
    image: wildfly_2
    ports:
    - 9002:8080
    networks:
      wildfly_network:

networks:
  wildfly_network:
    ipam:
      driver: default
```

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/docker-compose.png"/>
</div>

Donde podemos ver que tenemos 2 wildflys, uno en el puerto 9001 y 9002 comunicados por la subred wildfly_network.

Ahora al hacer:

```
sudo docker-compose up
```

Se nos desplegaría 2 aplicaciones en:

```
http://localhost:9001/app-web-ruben/
http://localhost:9002/app-web-ruben/
```

Teniendo una visión como por ejemplo:

<div align="center">
    <img src="../Imágenes/Clusterizando una App en Wildfly/Despliegue2.png"/>
</div>

Y tendríamos los dos despliegues.