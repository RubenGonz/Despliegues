# Dockerizar Wildfly

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/PortadaWildFly.png" width="400px"/>
    <img src="../Imágenes/Dockerizar Wildfly/PortadaDocker.png" height="250px"/>
</div>

## Índice

- [Introducción](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Dockerizar%20Wildfly.md#introducci%C3%B3n)
- [Creación de dockerfile](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Dockerizar%20Wildfly.md#creaci%C3%B3n-de-dockerfile)
- [Uso de la nueva imagen](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Dockerizar%20Wildfly.md#uso-de-la-nueva-imagen)

---

## Introducción

En esta práctica lo que haremos será agregar la configuración a nuestro servidor Wildfly debido a que tener una imagen de un servidor no implica que este esté configurado a nuestro gusto. Por ello para la realización de esta práctica deberemos tener con anterioridad una imagen de WildFly en nuestro docker, esto ya lo hemos hecho en la [instalación de WildFly en Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20WildFly%20en%20Docker.md).

---

## Creación de dockerfile

Lo primero que haremos para configurar nuestra imagen será crear el fichero donde tendremos guardada toda la información sobre la configuración que queramos aplicar.
Este lo pondremos en una carpeta situada, por ejemplo, en el directorio raíz de nuestro sistema, aunque su localización sea indiferente. Para ello haremos:

```console
mkdir wildfly-config
```

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/CrearCarpetaConf.png"/>
</div>

Y entraremos a ella con cd:

```console
cd wildfly-config
```

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/EntrarCarpetaConf.png"/>
</div>

Aquí dentro será donde crearemos nuestro fichero llamado "Dockerfile":

```console
touch Dockerfile
```

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/CrearDockerfile.png"/>
</div>

Y lo editaremos añadiendo:

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/EditarDockerfile.png"/>
</div>

~~~
# Base image
FROM jboss/wildfly:25.0.0.Final

# Maintainer
MAINTAINER "RubenGonz"

# Create user admin with password admin
RUN /opt/jboss/wildfly/bin/add-user.sh admin admin --silent

# Add custom configuration file
# ADD standalone.xml /opt/jboss/wildfly/standalone/configuration/

# Add example.war to deployments
# ADD example.war /opt/jboss/wildfly/standalone/deployments/

# JBoss ports
EXPOSE 8080 9990 8009

# Run
CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0", "-bmanagement", "0.0.0.0", "-c", "standalone.xml"]
~~~

El resultado debe ser como:

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/Dockerfile.png"/>
</div>

Ahora que ya tenemos este archivo con la configuración construiremos la imagen:

```console
sudo docker build -q --rm --tag=jboss/wildfly:25.0.0.Final-config .
```

El resultado que nos debe dar este comando debe ser una clave como la siguiente:

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/ConstruirImagen.png"/>
</div>

Para comprobar que lo hemos hecho correctamente usaremos el comando:

```console
sudo docker images
```

Donde si nos aparece la imagen lo habremos hecho correctamente como en este caso:

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/VerImagenes.png"/>
</div>

---

## Uso de la nueva imagen

Para usar nuestra imagen lo primero que debemos hacer es arrancarla como tal, esto lo haremos con el comando:

```console
sudo docker run -d -p 8080:8080 -p 9990:9990 -p 8009:8009 --name servidor-wilfly-config -it jboss/wildfly:25.0.0.Final-config
```

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/ArrancarImagen.png"/>
</div>

Con este comando estamos especificando que lo queremos arrancar en los puertos 8080, 9990 y 8009, que el nombre del servidor será "servidor-wilfly-config" y la imagen a usar.

Para comprobar que lo hemos ejecutado correctamente nos deberán salir las características que le hemos especificado al escribir:

```console
docker ps -a
```

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/CaracteristicasImagen.png"/>
</div>

Ahora que ya lo hemos hecho todo lo siguiente será comprobar si podemos acceder a la consola de administración de Wildfly debido a que si lo hubiésemos hecho mal no podríamos acceder a ella. Para ello lo que haremos será acceder a:

- localhost:8080

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/SalidaFinal.png"/>
</div>

Donde al pulsar en administración de consola nos debería dejar acceder mostrandonos:

<div align="center">
    <img src="../Imágenes/Dockerizar Wildfly/Administración.png"/>
</div>

Y ya con esto tendriamos administrado WildFly y podríamos tener un uso adecuado de la imagen.
