## Creación de los Pipeline en Java

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Requisitos]()

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

- [Enlace del proyecto en GitHub]()

Este contendrá la siguente estructura de ficheros:

<div align="center">
    <img src="../Imágenes/Creación de los Pipeline en Java/Portada.png"/>
</div>

Donde:

