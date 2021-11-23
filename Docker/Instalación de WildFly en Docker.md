# Instalación de WildFly en Docker

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/PortadaWildFly.png" width="400px"/>
    <img src="../Imágenes/Instalación de WildFly en Docker/PortadaDocker.png" height="250px" />
</div>

## Índice

- [Descarga de la Imagen]()
- [Arrancar contenedor]()
- [Comprobar instalación]()

---

## Descarga de la imagen

Lo primero para la instalación de Wildfly en Docker es descargar la versión que deseemos del propio sistema. En nuestro caso hemos elegido la versión 25.0.0.Final. Para descargarla ejecutaremos el comando docker pull:

```console
sudo docker pull jboss/wildfly:25.0.0.Final
```

El resultado debe ser similar a:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/DescargaImagen.png"/>
</div>

Para comprobar que lo hemos hecho correctamente al mostrar las imagenes de nuestro docker debería aparecer la nueva imagen:

```console
sudo docker images
```

En nuestro caso nos aparece en segunda posición:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/DockerImages.png"/>
</div>

---

## Arrancar contenedor

Ahora que ya tenemos nuestra imagen lista para usarse lo siguiente que haremos será arrancarla. Para hacer esto ejecutaremos:

```console
sudo docker run -d -p 5000:8080 --name "servidor-desa" jboss/wildfly
```

Con este comando:

~~~
- Arrancaremos el servidor en background (-d)
- Le especificaremos que lo ejecute en el puerto 5000:8080 (p 5000:8080) 
- Le daremos un alias para mayor comodidad (--name "servidor-desa").
~~~

La respuesta será:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/ArrancarImagen.png"/>
</div>

---

## Comprobar instalación

Para comprobar la instalación de ambos servicios podemos acceder a:

```console
ip a
```

Donde sí tenemos algo similar a esto lo habremos hecho correctamente:

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/ip-a.png"/>
</div>

Y para comprobar Wildfly podríamos acceder al puerto especificado anteriormente, en nuestro caso:

- localhost:5000

Donde veremos Wildfly desplegado.

<div align="center">
    <img src="../Imágenes/Instalación de WildFly en Docker/SalidaFinal.png"/>
</div>

Ahora ya podríamos usar WildFly teniendolo alojado en un contenedor Docker.