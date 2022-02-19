# Balanceo de cargas en Apache

<div align="center">
    <img src="../Imágenes/Balanceo de cargas en Apache/Portada.png"/>
</div>

## Índice

- [Introducción]()
- [Proxy inverso]()
- [Protocolos de Apache Proxy]()

---

## Introducción

En esta ocación lo que haremos será instalar y configurar una aplicación web sobre Apache que contará con 4 nodos incorporados en Docker para trabajar con balanceo de cargas y por lo tanto tener una mayor seguridad de que el servicio no caiga.

Para ello haremos uso de un proxy inverso que redijirá el tráfico de la aplicación en los distintos nodos que tengamos implantados, en nuestro caso 4. 

---

## Proxy inverso

El proxy inverso o servidor de paso se basa en una implantación que se hace añadiendo a la ecuación un servidor, en nuestro caso este servidor será Apache, esto se hace para que este nuevo servidor sea el encargado de la administración del tráfico de la web y centralice las autorizaciones entre otras.

El funcionamiento del proxy inverso que crearemos a continuación se basará en que las solicitudes mandadas por el cliente no llegarán directamente a los servidores backend, que son nuestros servidores principales y donde se encuentra toda la lógica de la aplicación, sino que se la mandará a un servidor intermedio que decidirá a cual de los distintos servidores backend mandar la solicitud repartiendo así el trabajo y aumentando la carga asumible del servidor a medida que se añaden más nodos.

---

## Requisitos

Para esta configuración haremos uso del proyecto encontrado en:

- [Proyecto Java](https://github.com/jpexposito/docencia/tree/master/COMUN/ejemplos/java/app-web-demo)
- [Instalación de Apache](https://github.com/RubenGonz/Despliegues/blob/main/Apache/Instalaci%C3%B3n%20de%20Apache2.md)
- [Instalación de Docker](https://github.com/RubenGonz/Despliegues/blob/main/Docker/Instalaci%C3%B3n%20de%20Docker.md)
- Instalación de Docker compose

---

## Modulos

Lo primero que haremos para poder aplicar un balanceador de cargas es cargar nuestros modulos.

- proxy
- proxy_http
- proxy_ajp
- rewrite
- deflate
- proxy_balancer
- proxy_connect
- proxy_html
- lbmethod_byrequests

Esto lo haremos con un comando como el siguiente:

```console
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_ajp
sudo a2enmod rewrite
sudo a2enmod deflate
sudo a2enmod headers
sudo a2enmod proxy_balancer
sudo a2enmod proxy_connect
sudo a2enmod proxy_html
sudo a2enmod lbmethod_byrequests
```

Nos faltaría reiniciar nuestro apache con:

```console
sudo systemctl restart apache2.service
```

---

## Docker-compose

Nuestro proyecto tendrá la estructura del proyecto base y lo que haremos será cambiar nuestro docker-compose dejandolo de la siguiente manera:

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
    - 8081:8080
    networks:
      - default

  wildfly2:
    build:
      context: .
      args:
        WILDFLY_NAME: wildfly_2
        CLUSTER_PW: secret_password
    image: wildfly_2
    ports:
    - 8082:8080
    networks:
      - default
  
  wildfly3:
    build:
      context: .
      args:
        WILDFLY_NAME: wildfly_3
        CLUSTER_PW: secret_password
    image: wildfly_3
    ports:
    - 8083:8080
    networks:
      - default
  
  wildfly4:
    build:
      context: .
      args:
        WILDFLY_NAME: wildfly_4
        CLUSTER_PW: secret_password
    image: wildfly_4
    ports:
    - 8084:8080
    networks:
      - default
```

---

## Configurar Apache

Nuestro apache necesita ser modificado para que se adapte a nuestra configuración y por ello accederemos a:

```console
sudo nano /etc/apache2/sites-available/000-default.conf
```

Donde escribiremos:

```
<Proxy balancer://mycluster>

    # Server 1
    BalancerMember http://localhost:8081

    # Server 2
    BalancerMember http://localhost:8082
    
    # Server 3
    BalancerMember http://localhost:8083

    # Server 4
    BalancerMember http://localhost:8084

</Proxy>

ProxyPass / balancer://mycluster/
```

Y para guardar los cambios en nuestro apache tendremos que hacer:

```console
sudo systemctl reload apache2
```

---

## Desplegar Docker-compose

Ahora si desplegaremos nuestra aplicación con:

```console
sudo docker-compose up -d
```

Podremos ver como tenemos los 4 nodos desplegados en sus puertos:

<div align="center">
    <img src="../Imágenes/Balanceo de cargas en Apache/Despliegue1.png"/>
</div>

<div align="center">
    <img src="../Imágenes/Balanceo de cargas en Apache/Despliegue2.png"/>
</div>

<div align="center">
    <img src="../Imágenes/Balanceo de cargas en Apache/Despliegue3.png"/>
</div>

<div align="center">
    <img src="../Imágenes/Balanceo de cargas en Apache/Despliegue4.png"/>
</div>