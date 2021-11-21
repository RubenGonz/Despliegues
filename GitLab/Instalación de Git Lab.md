# Instalación de Git Lab

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/Portada.png"/>
</div>

## Índice

- [¿Qué es?](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#qu%C3%A9-es)
- [Prerrequisitos de la instalación](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#prerrequisitos-de-la-instalaci%C3%B3n)
- [Instalación en local](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#instalaci%C3%B3n-en-local)
- [Instalación de paquetes adicionales](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#instalaci%C3%B3n-de-paquetes-adicionales)
- [Instalación de GitLab](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#instalaci%C3%B3n-de-gitlab)
- [Acceso](https://github.com/RubenGonz/Despliegues/blob/main/GitLab/Instalaci%C3%B3n%20de%20Git%20Lab.md#acceso)

---

## ¿Qué es?

GitLab es una herramienta de control de versiones y un gestor de repositorios muy popular en el mundo de la creación de aplicaciones.

---

## Prerrequisitos de la instalación

Para la instalación de GitLab habrá que tener un servidor Ubuntu con una cuenta superusuario.

---

## Instalación en local

Lo primero que debemos hacer para poder instalar GitLab es actualizar el sistema de paquetes que nos deja disponible Linux usando:

```console
sudo apt update && sudo apt upgrade
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/ActualizarPaquetes.png"/>
</div>

---

## Instalación de paquetes adicionales

A parte del propio software de GitLab tendremos que hacer la instalación de otros paquetes por lo que lo siguiente que haremos será:

```console
sudo apt install -y vim curl ca-certificates apt-transport-https
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/PaquetesAdicionales.png"/>
</div>

---

## Instalación de GitLab

Para la propia instalación podemos usar el repositorio APT asi que lo siguiente que haremos será ejecutar:

```console
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/InstalacionGitLab.png"/>
</div>

Una vez tengamos hecho esto lo siguiente será hacer:

```console
sudo apt install gitlab-ce
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/InstalacionGitLab-ce.png"/>
</div>

Para poder finalizar la instalación de este software adicional debemos configurar un archivo en específico, que es el siguiente:

```console
sudo gitlab-ctl reconfigure
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/ReconfigurarGitLab.png"/>
</div>

---

## Acceso

Para acceder a este servicio tendremos que acceder a nuestro navegador preferido y escribir:

- http://(nuestra ip)/

O por otra parte también podremos escribir:

- localhost

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/LocalHost.png"/>
</div>

Esto se debe a que es el puerto por defecto al instalar un servicio como es GitLab.

Para obtener las credenciales necesitaremos la información que está dentro de:

- /etc/gitLab/initial_root_password

Esto lo sabemos ya que al reconfigurar GitLab nos salio un mensaje que decia:

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/Credenciales.png"/>
</div>

Por ello ya sabemos que en este caso el usuario es root y las contraseña estará dentro de /etc/gitLab/initial_root_password.

Por ello accederemos haciendo:

```console
cd /etc/gitLab
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/EntrarGitLab.png"/>
</div>

Y dentro de aquí leeremos el archivo usando cat, por ejemplo:

```console
sudo cat initial_root_password
```

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/Contraseina.png"/>
</div>

Donde lo marcado en pantalla será la contraseña. Lo siguiente sería introducirla en la página web y ya estaríamos dentro.

<div align="center">
    <img src="../Imágenes/Instalación de Git Lab/Hall.png"/>
</div>
