# Fundamentos de un Pipeline Jenkins

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()
- [Que es un pipeline en Jenkins]()
- [Sintaxis de los pipelines en Jenkins]()
    - [Declarative]()
    - [Scripted]()
- [Creación de un Pipeline desde Jenkins]()
- [Ejemplos de Pipelines]()
    - [Java-Maven]()
    - [Node]()
    - [Ruby]()
    - [Python]()
    - [PHP]()
    - [Go]()
- [Vista general]()

---

## Introducción

En esta ocación lo que haremos será crear pipelines en Jenkins. Introduciremos lo que son, su finalidad y el funcionamientos de esta herramienta dentro de Jenkins junto a unos cuantos ejemplos, como la construcción de un hello-world en varios lenguajes.

---

## Requisitos

Para realizar esta práctica será necesario tener instalado Jenkins y Docker en nuestro sistema, para ello dispones de estos informes sobre la instalación de estas herramientas.

- [Instalación y configuración de Jenkins en Linux](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Linux.md)
- [Instalación y configuración de Jenkins en Docker](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Docker.md)
- [Instalación de Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md)

---

## Que es un pipeline en Jenkins

Los pipelines en Jenkins son una herramienta que sirven para implementar el despliegue continuo en una aplicación. Estas sirven conformando un conjunto de instrucciones sobre el proceso que vamos a evaluar desde que comienza la construcción de nuestra herramienta hasta que llega finalmente a un usuario. Con esto podemos comprobar si el proceso se completa con éxito o ver en que falla y así solucionar los problemas que tengamos.

Estos pipelines se escriben en un fichero llamado Jenkinsfile, que es donde escribimos las instrucciones que queremos testear para el despliegue.

Una de las muchas ventajas que nos proporciona tener pipelines de nuestra aplicación en Jenkins son:

- Los pipelines afectan a la construcción de todas las ramas y todos los pull request de nuestra aplicación.
- Nos facilitan la revisión del código.
- Centraliza la vista y la edición de este proceso para varios miembros del proyecto que son los encargados de tener el control del despligue.
- Nos permite varios tipos de sintaxis a gusto del usuario siendo más correcta la usada en un Jenkinsfile por encima de la que nos da la interfaz web.
- Nos permite el control del código aun contando con más versiones de la apliación ya que también podemos incorporar diferentes versiones de los Jenkinsfile.
- Los pipelines se ejecutan siempre que se reinicie la aplicación tanto de manera esperada como inesperada.
- Se puede pausar la ejecución o la frecuencia con la que se produce un pipeline.
- Se puede modificar el contenido de un pipeline para ampliarlo o adaptarlo a un proceso.

---

## Sintaxis de los pipelines en Jenkins

Los pipelines en Jenkins tienen dos tipo de sintaxis que son la "Declarative" y la "Scripted".

---

### Declarative

La sintaxis declarative tiene la siguiente estructura:

```
pipeline {
    agent any    
    stages {
        stage("Construir") {  
            steps {
                ... 
            }
        }
        stage("Probar") {  
            steps {
                ...  
            }
        }
        stage("Desplegar") {  
            steps {
                ...  
            }
        }
    }
}
```

Donde:

- agent any: Indica que el pipeline se ejecutará en cualquiera de los nodos.
- stage("nombreEtapa"): Define el lugar donde se declarará la etapa con el nombre entre comillas.
- ... : Aqui es donde se definiran los pasos a seguir por la etapa padre.

---

### Scripted

La sintaxis scripted tiene la siguiente estructura:

```
node {
    stage("Construir") {
        ...
    }
    stage("Probar") {
        ...
    }
    stage("Deploy") {
        ...
    }
}
```

En este caso:

- node: EIndica que el pipeline se ejecutará en cualquiera de los nodos
- stage("nombreEtapa"): Define el lugar donde se declarará la etapa con el nombre entre comillas. En el caso de los pipelines scripted, el bloque es opcional pero se recomienda su uso para facilitar la lectura del mismo entre otras cosas.
- ... : Aqui es donde se definiran los pasos a seguir por la etapa padre.

---

## Creación de un Pipeline desde Jenkins

Una vez que tengamos jenkins configurado a nuestro gusto lo que deberemos hacer es acceder al home de nuestra herramienta y dirijirnos a:

```
Nueva Tarea
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/NuevaTarea.png"/>
</div>

Dentro de aqui nos saldrán diversas opciones donde tendremos que elegir Pipeline y escribir el nombre de nuestro pipeline, en nuestro caso:

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/ElegirPipeline.png"/>
</div>

Ahora nos dirigirá a una pantalla donde pondremos la configuración de nuestro pipeline. Entre otras cosas tendremos la descripción de nuestro pipeline, de donde queremos extraer nuestro Jenkinsfile o en su lugar el apartado donde podriamos escribir nuestro pipeline entre otras cosas.

En nuestro caso escribiremos:

```
pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/Jenkinsfile.png"/>
</div>

Ahora tendremos que guardar mandandonos a una pantalla como la siguiente:

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/HomePipeline.png"/>
</div>

Posiblemente tengamos problemas debido a que:

Alojandolo en docker/docker-compose: Nuestro Jenkins no reconoce docker en su sistema. 

Alojandolo en local: No tenemos los plugins de *Docker Pipeline* o *Docker plugin* y Jenkins no tiene permisos de Docker.

Para este segundo caso solo tendremos que dirijirnos a "Administrar Jenkins" desde el panel de control, posteriormente buscar la opción "Administrar Plugins", pulsar en "Todos los plugins" y desde aqui buscar e instalar los plugins normbrados. Posteriormente deberemos ejecutar en nuestra terminal:

```console
sudo gpasswd -a jenkins docker
newgrp docker
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/AniadirUsuario.png"/>
</div>

Con esto añadiremos jenkins al grupo de docker y posteriormente tendremos que parar y arrancar, o en su lugar reiniciar jenkins con:

```console
sudo systemctl restart jenkins.service
```

Ahora ya prodiamos construir nuestro pipeline desde esta opción:

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/ReiniciarJenkins.png"/>
</div>

Donde si nos vamos a la ejecución de nuestro pipeline despues de haber finalizado podremos irnos a la parte baja de nuestra página del pipeline donde veremos:

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/SalidaMavenLog.png"/>
</div>

---

## Ejemplos de Pipelines

Como ya hemos visto podemos usar docker a nuestro antojo y comprobar el buen funcionamiento de nuestra instalación gracias a los pipelines asi que ahora lo que haremos será comprobarlo con más herramientas

---

### Java-Maven

```
pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionMaven.png"/>
</div>

---

### Node

```
pipeline {
    agent { docker { image 'node:16.13.1-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionNode.png"/>
</div>

---

### Ruby 

```
pipeline {
    agent { docker { image 'ruby:3.0.3-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'ruby --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionRuby.png"/>
</div>

---

### Python  

```
pipeline {
    agent { docker { image 'python:3.10.1-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'python --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionPython.png"/>
</div>

---

### PHP  

```
pipeline {
    agent { docker { image 'php:8.1.0-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionPHP.png"/>
</div>

### Go  

```
pipeline {
    agent { docker { image 'golang:1.17.5-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'go version'
            }
        }
    }
}
```

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VersionGo.png"/>
</div>

## Vision general

Para poder tener una vision general de todos nuestros pipelines podemos acceder a nuestro Home Jenkins donde en la parte inferior tendremos un mapa de nuestros pipelines indicando cuales han fallado y su tiempo de ejecución entre muchos otros detalles.

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/VisionGeneral.png"/>
</div>