# Dockerizando Node.js

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/Portada.png"/>
</div>

## Índice

- [Requisitos]()
- [Creación de la aplicación]()
- [Creación del DockerFile]()
- [Creación de la imagen]()
- [Ejecución de la imagen]()
- [Comprobación]()

---

## Requisitos

Lo primero para el uso de un servidor Node.js en Docker es contar previamente con docker instalado en nuestro sistema debidamente configurado a nuestro gusto, por ello le facilitaremos este enlace donde podrá [instalar Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md). 

Esta instalación la haremos correspondiendo con la versión 20.04 de Ubuntu.

---

## Creación de la aplicación

Lo primero que haremos será crear un directorio vacío donde tendremos nuestro entorno de trabajo para manejar nuestros archivos con el nombre que decidamos como por ejemplo en nuestro caso:

```console
mkdir Node.js-Folder
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CarpetaNode.png"/>
</div>

Lo siguiente que haremos será meternos en la carpeta creada y empezar a crear los archivos necesarios empezando por "package.json" que contendrá información sobre nuestra app como las dependencias.

```console
cd Node.js-Folder
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/EntrarNode.png"/>
</div>

```console
nano package.json
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/EditarPackage.png"/>
</div>

Dentro de este archivo pondremos la siguiente información:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/Package.png"/>
</div>

Viendo que tenemos aquí el nombre, la versión que tenemos…

Lo siguiente que haremos será usar "npm install" pero tendremos que hacerlo en el caso de que tengamos una versión de "npm" superior a la 5 por lo que para asegurarnos lo que haremos será mirar que versión tenemos instalada con:

```console
npm --version
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/VersionNpm.png"/>
</div>

En el caso de que nuestra versión no fuese superior tendremos que actualizar los paquetes con:

```console
sudo apt update
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/ActualizarPaquetes.png"/>
</div>

Y ahora instalarnos el paquete actualizado con:

```console
sudo apt install npm
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/InstalarPaqueteNpm.png"/>
</div>

Y ahora si que podriamos usarlo:

```console
npm install
```

Que nos mostraría:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/InstalarNpm.png"/>
</div>

Ahora, lo siguiente que haremos será crear el archivo "server.js" que contendrá información sobre variables como el puerto que usaremos para alojarlo. Antes de esto vamos a instalar a través de npm una herramienta que necesitaremos, solo tendremos que ejecutar:

```console
npm install express --save
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/InstalarExpress.png"/>
</div>

Ahora sí, crearemos el archivo server.js haciendo:

```console
nano server.js
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CrearServer.png"/>
</div>

Dentro de este archivo pondremos la siguiente información:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/Server.png"/>
</div>

---

## Creación del DockerFile

Lo siguiente que haremos será crear el archivo de configuración para que Docker pueda manejar la imagen a nuestro gusto.

```console
touch Dockerfile
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CrearDockerfile.png"/>
</div>

Antes de ponernos a editar el archivo Dockerfile vamos a crear nuestro directorio de trabajo donde trabajariamos con Node.js, en nuestro caso será:

```console
mkdir web
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CrearWeb.png"/>
</div>

Ahora si, vamos a editar la configuración que le queremos dar al dockerfile. Para ello lo editaremos poniendo:

```console
nano Dockerfile
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/EditarDockerfile.png"/>
</div>

Y empezaríamos a escribir:

- Que queremos que contenga la imagen por lo que pondremos node y la versión así:

```console
FROM node:16
```

- La carpeta que funcionará como directorio de trabajo para Node.js: Esta carpeta no la hemos creado así que haremos:

```console
WORKDIR /home/rubengonz/Node.js-Folder/web
```

- Los ficheros creados con anterioridad, solo tendremos que hacer:

```console
COPY package*.json ./
```

- E indicar que vuelva a ejecutar:

```console
RUN npm install
```

- Para manejar con más comodidad el código haremos:

```console
COPY . .
```

- Lo siguiente será indicar que puerto queremos exponer:

```console
EXPOSE 8080
```

- Y por último indicar con que queremos arrancar el server:

```console
CMD [ "node", "server.js" ]
```

El aspecto tras editarlo debe quedar similar a:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/Dockerfile.png"/>
</div>

Para manejarnos mejor con Docker crearemos el archivo ".dockerignore" donde pondremos los archivos que queremos que Docker ignore.

```console
nano .dockerignore
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CrearDockerIgnore.png"/>
</div>

Los archivos que pondremos serán:

```console
node_modules
npm-debug.log
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/DockerIgnore.png"/>
</div>

---

## Creación de la imagen

Ahora que ya tenemos nuestro Node configurado lo siguiente será crear nuestra imagen de Docker con Node.js. Para ello lo que haremos será:

```console
sudo docker build . -t node-web
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/ImagenDocker.png"/>
</div>

Con esto ya tendremos la imagen creada y lo podremos comprobar con:

```console
sudo docker images
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/ImagenesDocker.png"/>
</div>

---

## Ejecución de la imagen

para la ejecución de la imagen lo que haremos será asignarle a la imagen la ip y el puerrto indicado con anterioridad en los anteriores ficheros de esta manera:

```console
docker run -p 8080:8080 -d node-web
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/AsignarPuertos.png"/>
</div>

Para comprobar que está y se puede ejecutar ejecutaremos unas líneas que pusimos con anterioridad en package.json que es poner:

```console
sudo docker logs (id del contenedor)
```

Donde la id la averiguaremos usando:

```console
sudo docker ps
```

Siendo la respuesta la siguiente:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/DockerLog.png"/>
</div>

---

## Comprobación

Para comprobar que lo hemos hecho todo correctamente podemos usar la herramienta curl, que en el caso de no tenerla instalada solo tendríamos que ejecutar:

```console
sudo apt-get install curl
```

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/InstalarCurl.png"/>
</div>

Para comprobar que la tenemos instalada podemos ejecutar:

```console
curl --version
```

Donde nos debe aparecer algo similar a:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/VersionCurl.png"/>
</div>

Ahora que ya contamos con la herramienta lo siguiente que haremos será ejecutar:

```console
curl -i (puerto indicado)
```

Donde si hemos seguido los pasos al pie de la letra nos saldrá este mensaje:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/CurlPuerto.png"/>
</div>

Con esto ya sabemos que contamos con nuestro entorno Node.js y en el caso de que quisiéramos ver nuestra interfaz deberemos acceder al puerto indicado a través del navegador donde nos saldrá:

<div align="center">
    <img src="../Imágenes/Dockerizar Node.js/SalidaNavegador.png"/>
</div>

Con esto ya podrá diseñar su propio entorno en Node.js usando Docker.
