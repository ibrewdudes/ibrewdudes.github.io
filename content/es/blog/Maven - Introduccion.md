---
title: "Maven: introducción"
description: "Uso básico de maven"
date: 2023-10-21T00:00:37+02:00
tags: ["java", "maven", "basico"]
thumbnail: /topics/java.jpg
---

[Maven](https://maven.apache.org/) es una herramienta para la gestión de proyectos java. Con maven podemos:
* Especificar las librerías que necesita el proyecto (dependencias)
* Compilar, empaquetar y ejecutar el proyecto
* Usar plugins para ampliar las capacidades del propio maven que nos permiten hacer cosas como generar la documentación del código o generar código automáticamente

La configuración de un proyecto maven se hace mediante un fichero XML llamado **POM** (Project Object Model)

## Instalación de maven
### Instalación en Linux
La forma más rápida es instalarlo desde el repositorio de nuestra distribución:
```bash
# Fedora
dnf install maven
   
# Debian/ubuntu
apt-get install maven
```
Podemos instalarlo manualmente descargando los binarios desde la [web de apache](https://maven.apache.org/download.cgi). Lo descomprimimos en un directorio y añadimos al fichero **.bashrc**:
* La variable de entorno **MAVEN_HOME** con el directorio donde lo hemos descomprimido
* El directorio con los binarios al path del sistema

![.bashrc](/maven/maven-bashrc-config.png ".bashrc")

Podemos comprobar si la configuración es correcta ejecutando:
```
mvn -version
```

![Versión de maven](/maven/maven-mvn-version.png "Versión de maven")

### Instalación en Windows
Descargamos los binarios desde la [web de apache](https://maven.apache.org/download.cgi) y hacemos lo mismo que en linux:
* Creamos la variable de entorno **MAVEN_HOME** con el directorio raíz donde lo hemos descomprimido
* Añadimos el directorio con los binarios al path del sistema

## Creación de un proyecto básico
Podemos crear un proyecto maven desde consola usando una plantilla (un arquetipo en la nomenclatura de maven). El siguiente ejemplo crea un proyecto simple utilizando el arquetipo **quickstart**:

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
* Arquetipo: **quickstart**
* Versión del arquetipo: 1.4
* Crear el proyecto de manera interactiva: no

Maven maneja el concepto de **artefacto**, que se refiere a cualquier elemento generado por maven. El concepto de artefacto se suele aplicar a los ficheros **.jar** que se crean al compilar un proyecto java pero pueden ser otro tipo de ficheros. Un artefacto maven se identifica por sus coordenadas **GAV**:
* **G**rupo: **org.rlnieto.app**
* **A**rtefacto: **pruebas**
* **V**ersión: no la hemos especificado pero el arquetipo nos creará por defecto la versión **1.0-SNAPSHOT**. Es habitual que las versiones se clasifiquen como snapshot o release. Las snapshot son versiones temporales que cambian con frecuencia durante el desarrollo y las versiones release son las estables, pensadas para ser usadas en producción

Si abrimos el proyecto que hemos creado con [IntelliJ](https://www.jetbrains.com/idea/download/) veremos que tiene la siguiente estructura:

![Estructura de un proyecto maven](/maven/maven-estructura-proyecto.png "Estructura de un proyecto maven")

Los proyectos maven tienen todos la misma estructura de directorios:

```
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── org
    │           └── rlnieto
    │               └── app
    │                   └── App.java
    └── test
        └── java
            └── org
                └── rlnieto
                    └── app
                        └── AppTest.java
```

En el directorio **src/main/java**, del que van a colgar todos los paquetes java, tenemos los fuentes de la aplicación y en **src/test/java** las clases para los test. En la raíz tenemos el fichero **pom.xml** que contiene los detalles de configuración del proyecto:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.rlnieto.app</groupId>
  <artifactId>pruebas</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>pruebas</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

### El fichero **pom.xml**
En el fichero que ha creado el arquetipo **quickstart** vemos los siguientes apartados:
* Datos generales del proyecto
    ```xml
    <!-- Coordenadas GAV -->
    <groupId>org.rlnieto.app</groupId>
    <artifactId>pruebas</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>pruebas</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>
    
    ```
* Propiedades, son valores que se van a usar en otras secciones del pom o en los plugins que utilicemos
    ```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>
    ```
* Dependencias, donde se definen los artefactos que va a utilizar nuestra aplicación. Por ejemplo, si vamos a acceder a una BD de [MariaDB](https://mariadb.org/) incluiríamos una dependencia para poder utilizar las librerías de conexión a la BD. El arquetivo que hemos usado nos incluye solamente la dependencia de [JUnit](https://junit.org/junit5/), que se utiliza para las pruebas
    ```xml
    <dependencies>
        <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
        </dependency>
    </dependencies>
    ```
* Build, que contiene los elementos necesarios para compilar y empaquetar la aplicación como un artefacto maven. El arquetipo que hemos usado sólo ha añadido algunos plugins básicos que nos van a permitir por ejemplo compilar el proyecto. Existen multitud de plugins para hacer cosas como validar la calidad del código o generar clases automáticamente

## Compilación y empaquetado
Las dos operaciones más habituales que realizaremos serán compilar el proyecto y empaquetarlo y podemos hacerlo desde la línea de comandos o desde el IDE

### Compilación del proyecto
Lo compilamos con el comando:
```bash
mvn clean compile
```

En realidad estamos ejecutando dos comandos (dos **goals**):
* clean: elimina todos los ficheros generados en compilaciones anteriores
* compile: compila el proyecto

Si miramos el directorio del proyecto veremos que ha aparecido un nuevo subdirectorio llamado **target** con los ficheros generados durante la compilación:
```
target/
├── classes
│   └── org
│       └── rlnieto
│           └── app
│               └── App.class
├── generated-sources
│   └── annotations
└── maven-status
    └── maven-compiler-plugin
        └── compile
            └── default-compile
                ├── createdFiles.lst
                └── inputFiles.lst

```

### Empaquetado
Lo hacemos con el comando:
```bash
mvn clean install
```

Si miramos el directorio target veremos que ha aparecido nuevos fichero:
```
target
├── classes
│   └── org
│       └── rlnieto
│           └── app
│               └── App.class
├── generated-sources
│   └── annotations
├── generated-test-sources
│   └── test-annotations
├── maven-archiver
│   └── pom.properties
├── maven-status
│   └── maven-compiler-plugin
│       ├── compile
│       │   └── default-compile
│       │       ├── createdFiles.lst
│       │       └── inputFiles.lst
│       └── testCompile
│           └── default-testCompile
│               ├── createdFiles.lst
│               └── inputFiles.lst
├── pruebas-1.0-SNAPSHOT.jar
├── surefire-reports
│   ├── org.rlnieto.app.AppTest.txt
│   └── TEST-org.rlnieto.app.AppTest.xml
└── test-classes
    └── org
        └── rlnieto
            └── app
                └── AppTest.class

```

El que nos interesa es **pruebas-1.0-SNAPSHOT.jar**, que contiene nuestra aplicación empaquetada

Además de crear el fichero .jar lo ha copiado en el directorio **.m2/repository** del home de nuestro usuario. Cuando maven compila un proyecto busca las dependencias en este directorio y de esta manera nuestro artefacto estará disponible para ser usado por otros proyectos java

## El directorio .m2 y el fichero settings.xml
El directorio **.m2** funciona como una cache de artefactos maven. Cuando compilamos un proyecto java maven busca primero en este directorio las dependencias que necesita y si no las encuentra ahí las busca en un repositorio remoto llamado [central](https://central.sonatype.com/?smo=true), pero es posible especificar otros repositorios en el fichero **.m2/settings.xml**

## Ejecutando nuestro proyecto de ejemplo
Para ejecutar el proyecto que hemos creado tenemos que hacer una pequeña modificación para indicar cuál va a ser la clase principal, la que contiene el método **main()** y que será la que se lance cuando ejecutemos el fichero .jar. Para ello modificamos la definición del plugin **maven-jar-plugin** en el fichero **pom.xml**:
```xml
<plugin>
  <artifactId>maven-jar-plugin</artifactId>
  <version>3.0.2</version>
  <configuration>
    <archive>
      <manifest>
        <packageName>org.rlnieto.app</packageName>
        <mainClass>org.rlnieto.app.App</mainClass>
      </manifest>
      <manifestEntries>
        <Created-By>rlnieto</Created-By>
      </manifestEntries>
    </archive>
  </configuration>
</plugin>
``` 

Lo que hará el plugin es crear un fichero **MANIFEST.MF** con los datos de la clase que hay que ejecutar y lo incluirá en el fichero jar

Para ver cómo funciona ejecutamos en una consola:
```bash
# Compilamos y generamos el fichero .jar
mvn clean compile

# Ejecutamos el fichero .jar
java -jar target/pruebas-1.0-SNAPSHOT.jar
```

Y veremos la salida de la aplicación:
![Ejecución del fichero jar](/maven/ejemplo-salida-maven.png "Salida de la ejecución")