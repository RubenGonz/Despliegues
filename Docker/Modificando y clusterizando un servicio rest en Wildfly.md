# Modificando y clusterizando un servicio rest en Wildfly

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/Portada.png"/>
</div>

---

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Modificando%20y%20clusterizando%20un%20servicio%20rest%20en%20Wildfly.md#introducci%C3%B3n)
- [Requisitos](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Modificando%20y%20clusterizando%20un%20servicio%20rest%20en%20Wildfly.md#requisitos)
- [Creación del proyecto](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Modificando%20y%20clusterizando%20un%20servicio%20rest%20en%20Wildfly.md#creaci%C3%B3n-proyecto)
- [Ejecución y vista final](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Modificando%20y%20clusterizando%20un%20servicio%20rest%20en%20Wildfly.md#ejecuci%C3%B3n-y-vista-final)

---

## Introducción

Lo que haremos esta ocasión va a ser juntar varias de las tareas anteriores como son el LAMP en docker y el cluster en Wildfly.

---

## Requisitos

Para este ejercicio haremos uso de las herramientas usadas en:

- [Instalación de Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md)
- Instalación de Docker-compose
- [Instalación de wildfly en docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20WildFly%20en%20Docker.md)
- [Cluster en WildFly](https://github.com/RubenGonz/Despliegues/blob/main/WildFly/Clusterizando%20una%20App%20en%20Wildfly.md)
- [LAMP en docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/LAMP%20en%20Docker.md)

---

## Creación proyecto 

Nuestro proyecto base lo tomaremos del siguiente enlace:

- [Proyecto](https://github.com/wildfly/quickstart/tree/main/helloworld-rs)

A este proyecto le haremos unos cuantos cambios como por ejemplo cambiar la estructura del proyecto a una como la siguiente:

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/Estructura.png"/>
</div>

Las carpetas conf y dump las habremos obtenido de la práctica de la creación de una estructura LAMP, la carpeta src la tenemos de nuestro proyecto base, como el pom.xml, la carpeta target la nombraremos posteriormente y los ficheros dockerfile y docker-compose.yml los crearemos a continuación.

El fichero Dockerfile será como el siguiente:

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

El fichero docker-compose.yml será como el siguiente:

```
version: "3.5"
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
    wildfly3:
        build:
            context: .
            args:
                WILDFLY_NAME: wildfly_3
                CLUSTER_PW: secret_password
        image: wildfly_3
        ports:
        - 9003:8080
        networks:
            wildfly_network:
    db:
        image: mysql:8.0
        ports:
            - "3306:3306"
        command: --default-authentication-plugin=mysql_native_password
        environment:
            MYSQL_DATABASE: dbname
            MYSQL_USER: ruben
            MYSQL_PASSWORD: test
            MYSQL_ROOT_PASSWORD: test
        volumes:
            - ./dump:/docker-entrypoint-initdb.d
            - ./conf:/etc/mysql/conf.d
            - persistent:/var/lib/mysql
        networks:
            - default
    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        links:
            - db:db
        ports:
            - 9000:80
        environment:
            MYSQL_USER: ruben
            MYSQL_PASSWORD: test
            MYSQL_ROOT_PASSWORD: test
volumes:
    persistent:
networks:
    wildfly_network:
        ipam:
            driver: default
```

Donde tenemos 3 instancias del servicio wildFly, una base de datos y el phpmyadmin.

El pom.xml lo modificaremos en la sección:

En la sección build:

```
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
```

Y en la sección properties:

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <jetty.port>9000</jetty.port>
    <maven-war-plugin-version>3.3.1</maven-war-plugin-version>
    <jetty-maven-plugin-version>11.0.7</jetty-maven-plugin-version>
    <maven-compiler-plugin-version>3.8.0</maven-compiler-plugin-version>
    <version.server.bom>26.0.0.Final</version.server.bom>
</properties>
```

---

## Ejecución y vista final

Para la creación de la carpeta target deberemos ejecutar los siguientes comandos:

```
mvn clean install
```

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/CleanInstall.png"/>
</div>

Que nos creará la carpeta y posteriormente:

```
mvn clean jetty:run
```

Que nos lo correrá en el puerto 9000.

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/Jetty.png"/>
</div>

Ahora si en el caso de que todo haya ido correctamente lo siguiente que haremos será ejecutar nuestro docker-compose con:

```
sudo docker-compose up -d
```

Lo que haría que en el puerto 9000 tendremos nuestro phpMyAdmin con la base de datos:

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/phpMyAdmin.png"/>
</div>

Y en los puertos 9001, 9002 y 9003 tendríamos nuestro Wildfly desplegado:

<div align="center">
    <img src="../Imágenes/Modificando y clusterizando un servicio rest en Wildfly/WildFly.png"/>
</div>

Y con esto tendríamos nuestro despliegue completo.