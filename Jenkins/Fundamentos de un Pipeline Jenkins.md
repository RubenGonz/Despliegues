# Fundamentos de un Pipeline Jenkins

<div align="center">
    <img src="../Imágenes/Fundamentos de un Pipeline Jenkins/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

---

## Introducción

En esta ocación lo que haremos será crear pipelines en Jenkins. Introduciremos lo que son, su finalidad y el funcionamientos de esta herramienta dentro de Jenkins junto a unos cuantos ejemplos, como la construcción de un hello-world en varios lenguajes.

---

## Requisitos

Para realizar esta práctica será necesario tener instalado Jenkins en nuestro sistema, para ello dispones de estos informes sobre la instalación de Jenkins.

- [Instalación y configuración de Jenkins en Linux](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Linux.md)
- [Instalación y configuración de Jenkins en Docker](https://github.com/RubenGonz/Despliegues/blob/main/Jenkins/Instalaci%C3%B3n%20y%20configuraci%C3%B3n%20de%20Jenkins%20en%20Docker.md)

---

## Que es un pipeline en Jenkins

Los pipelines en Jenkins son una herramienta que sirven para implementar el despliegue continuo en una aplicación. Estas sirven conformando un conjunto de instrucciones sobre el proceso que vamos a evaluar desde que comienza la construcción de nuestra herramienta hasta que llega finalmente a un usuario. Con esto podemos comprobar si el proceso se completa con éxito o ver en que falla y así solucionar los problemas que tengamos.

Estos pipelines se escriben en un fichero llamado Jenkinsfile, que es donde escribimos las instrucciones que queremos testear para el despliegue.

Una de las muchas ventajas que nos proporciona tener pipelines de nuestra aplicación en Jenkins son:

- Los pipelines afectan a la construcción de todas las ramas y todos los pull request de nuestra aplicación.
- Nos facilitan la revisión del código.
- Centraliza la vista y la edición de este proceso para varios miembros del proyecto que son los encargados de tener el control del despligue.
- Nos permite varios tipos de sintaxis a gusto del usuario siendo más correcta la usada en un Jenkinsfile por encima de la que nos da la interfaz web.
- Nos permite el control del código aun contando con más versiones de la apliación ya que también podemos incorporar diferentes versiones de los JenkinsFile.
- Los pipelines se ejecutan siempre que se reinicie la aplicación tanto de manera esperada como inesperada.
- Se puede pausar la ejecución o la frecuencia con la que se produce un pipeline.
- Se puede modificar el contenido de un pipeline para ampliarlo o adaptarlo a un proceso.

## Sintaxis de los pipelines en Jenkins

Los pipelines en Jenkins tienen dos tipo de sintaxis que son la "Declarative" y la "Scripted".

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
- stage("nombreEtapa"): Define el lugar donde se declarará la etapa con el nombre entre comillas. En el caso de los pipelines scripted, el bloque es opcional pero se recomienda su uso para facilitar la lectura del mismo.
//: Esta parte contiene cada uno de los pasos a realizar en la etapa de “Construir”.