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

## Protocolos de Apache Proxy

