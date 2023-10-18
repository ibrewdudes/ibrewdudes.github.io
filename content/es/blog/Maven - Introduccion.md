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
### Instalación en Linux
La forma más rápida es instalarlo desde el repositorio de nuestra distribución:
```bash
# Fedora
dnf install maven
   
# Debian/ubuntu
apt-get install maven
```
Podemos instalarlo manualmente descargando lso binarios desde la [web de apache](https://maven.apache.org/download.cgi). Lo descomprimimos en un directorio y añadimos al fichero **.bashrc**:
* La variable de entorno **MAVEN_HOME** con el directorio raíz donde lo hemos descomprimido
* El directorio con los binarios al path del sistema

![Instalar maven](/maven/maven-bashrc-config.png "Instalación de maven")

Podemos comprobar si la configuración es correcta ejecutando:
```
mvn -version
```

![Versión de maven](/maven/maven-mvn-version.png "Versión de maven")

### Instalación en Windows
Descargamos los binarios desde la [web de apache](https://maven.apache.org/download.cgi) y, al igual que en linux:
* Creamos la variable de entorno **MAVEN_HOME** con el directorio raíz donde lo hemos descomprimido
* Añadimos el directorio con los binarios al path del sistema

## Creación de un proyecto básico
Podemos crear un proyecto maven desde consola usando una plantilla. El siguiente ejemplo crea un proyecto simple utilizando la plantilla **quickstart**:

```
mvn  archetype:generate \
    -DgroupId=org.rlnieto.app \
    -DartifactId=pruebas \
    -DarchetypeArtifactId=maven-archetype-quickstart \
    -DarchetypeVersion=1.4 \
    -DinteractiveMode=false
```

Los parámetros que tenemos que pasar a maven son:
* Grupo: **org.rlnieto.app**
* Artefacto: **pruebas**
* Arquetipo (plantilla) a utilizar: **quickstart**
* Versión del arquetipo: 1.4
* Crear el proyecto de manera interactiva: no (para que no nos vaya pidiendo más datos)

Maven maneja el concepto de **artefacto**, que se refiere a cualquier elemento compilado por maven. El concepto de artefacto se suele aplicar a los ficheros **.jar** que se generan al compilar un proyecto java pero pueden ser otro tipo de ficheros. Un artefacto maven se identifica por sus coordenadas **GAV**:
* **G**rupo: **org.rlnieto.app**
* **A**rtefacto: **pruebas**
* **V**ersión: no la hemos especificado pero el arquetipo nos creará por defecto la versión **1.0-SNAPSHOT**. Es habitual que las versiones se clasifiquen como snapshot o release. Las versiones snapshot son versiones temporales que cambian con frecuencia durante el desarrollo y las versiones release son las estables, pensadas para ser usadas en producción. 

Si abrimos el proyecto que hemos creado con [IntelliJ](https://www.jetbrains.com/idea/download/) veremos que tiene la siguiente estructura:

![Estructura de un proyecto maven](/maven/maven-estructura-proyecto.png "Estructura de un proyecto maven")