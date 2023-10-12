---
title: "Camel: introducción"
description: "Conceptos básicos de camel"
date: 2023-09-07T00:00:37+02:00
tags: ["contenedores", "podman"]
thumbnail: /topics/camel.jpg
---

Un contenedor es un objeto que nos permite ejecutar una aplicación de manera aislada. Conceptualmente es similar a una máquina virtual pero mucho más ligera.

Por ejemplo, podemos ejecutar en local una aplicación como el [editor de swagger](https://editor.swagger.io/) sin necesidad de hacer una instalación tradicional

Para manejar los contenedores (crearlos, descargarlos, ejecutarlos...) necesitamos una herramienta como docker o podman

# Instalación de podman
En linux lo podemos instalar con el gestor de paquetes de nuestra distribución. 

En windows es un poco más complicado porque  los contenedores utilizan características del kernel de linux y es necesario crear una máquina virtual para correr linux:
- Descargamos [podman](https://github.com/containers/podman/releases)
- Dejamos marcada la opción de instalar WSL

![Instalar podman](/images/contenedores/instalacion-podman-wsl.png "Instalación de podman")

- Una vez terminada la instalación se reiniciará el equipo
- Abrimos powershell y tecleamos:
  - podman machine init -> descargará una versión mínima de Fedora
  - podman machine start -> arranca la máquina virtual que nos permitirá ejecutar podman

# Ejecución de un contenedor
Sólo tenemos que teclear el siguiente comando desde una consola:

    podman run \
         -d \
         -p 8081:8080 \
        --rm \
        --name swagger-editor swaggerapi/swagger-editor

Nos preguntará desde dónde queremos descargar la aplicación:
![Descarga de la imágen de swagger](/images/contenedores/podman-swagger-editor-image-registries.png)

Elegimos **docker.io** y cuando termine la descarga accedemos a la url <http://localhost:8081>:
![Swagger editor](/images/contenedores/swagger-editor-screenshot.png)

# Un poco de teoría
¿Qué acabamos de hacer? Veamos lo que significa el comando que acabamos de ejecutar:

    podman run \
         -d \
         -p 8081:8080 \
        --rm \
        --name swagger-editor \
        swaggerapi/swagger-editor

- **run**: ejecuta un contenedor a partir de una imágen, si la imágen no existe en la máquina local la descarga. La relación que existe entre una imágen y un contenedor es la misma que existe entre una clase y una instancia: la imágen es el 'molde' a partir del cual podemos crear varios contenedores
- **-d**: crea el contendor en segundo plano. Si no usamos esta opción la consola se quedará bloqueada hasta que detengamos el contenedor
- **-p 8081:8080**: mapea hacia el puerto local 8081 el puerto 8080 del contenedor
- **--rm**: borra (remove) el contenedor una vez detenido
- **--name swagger-editor**: crea el contenedor con el nombre swagger-editor
- **swaggerapi/swagger-editor**: descarga la imágen con este nombre desde **docker.io**. Las imágenes están almacenadas en **registros** y esta la hemos descargado desde [Docker Hub](https://hub.docker.com/r/swaggerapi/swagger-editor). Otro registros públicos son [Quay](https://quay.io/) o el del proyecto [Fedora](https://registry.fedoraproject.org/)

Podemos ver la imágen que hemos descargado con el comando:

    podman images

![Listado de imágenes](/images/contenedores/images-list.png)

A partir de esta imagen podemos crear otros contenedores:

    podman run \
         -d \
         -p 8091:8080 \
        --rm \
        --name otro-swagger-editor \
        swaggerapi/swagger-editor

Al que podemos acceder desde la url <http://localhost:8091>

Podemos ver los contenedores que tenemos levantados con el comando:
    podman ps

![Lista de contenedores activos](/images/contenedores/lista-contenedores.png)

Y podemos pararlos con:

    podman stop otro-swagger-editor
    podman stop swagger-editor

Si ahora hacemos:

    podman ps

Veremos que ya no hay contedores activos. Pero si lanzamos el comando:

    podman images

Veremos que la imagen sigue existiendo

:warning: Si usas podman en windows tendrás que iniciar la VM de podman cada vez que arranques el sistema mediante el comando:

    podman machine start
