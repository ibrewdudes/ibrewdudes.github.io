---
title: "Maven: introducción"
description: "Uso básico de maven"
date: 2023-10-15T00:00:37+02:00
tags: ["java", "maven"]
thumbnail: /topics/java.jpg
draft: yes
---

[Maven](https://maven.apache.org/) es una herramienta para la gestión de proyectos java. Con maven podemos:
* Especificar las librerías que necesita el proyecto (dependencias)
* Compilar, empaquetar y ejecutar el proyecto
* Usar plugins para ampliar las capacidades del propio maven que nos permiten hacer cosas como generar la documentación del código o generar código automáticamente

La configuración de un proyecto maven se hace mediante un fichero XML llamado POM (Project Object Model) 

## Instalación de maven
La forma más rápida es instalarlo desde el repositorio de nuestra distribución:
```bash
# Fedora
dnf install maven
   
# Debian/ubuntu
apt-get install maven
```

En windows tendremos que descargarlo desde la [web de apache]()

## Creación de un proyecto básico

