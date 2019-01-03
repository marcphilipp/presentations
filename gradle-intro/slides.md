---
title: Gradle
theme: white
highlightTheme: atom-one-light
css: /assets/gradle.css
revealOptions:
    transition: zoom
    slideNumber: false
    controls: false
    markdown:
        smartypants: true
        breaks: true
---

<!-- .slide: data-background="/assets/gradle-title.jpg" data-transition="fade" -->
# Gradle -- Build Happiness
<!-- .element style="font-size:160%; margin-top:450px" -->

### Marc Philipp, Gradle Inc.

----

## ![Marc Philipp](/assets/marc-philipp.jpg) <!-- .element style="width:25%; float:right;" --> Marc Philipp

*Software Engineer bei Gradle, Inc.*

JUnit Maintainer seit 2012

*Twitter:* [@marcphilipp](https://twitter.com/marcphilipp)
*Web:* [marcphilipp.de](https://www.marcphilipp.de)

----

# ✋

## Show of Hands

----
<!-- .slide: data-background-color="#01303a" -->
# Was ist Gradle?

----

## Was ist Gradle?

> Gradle ist ein Build- und Automatisierungswerkzeug.

* Basiert auf der Java Virtual Machine (JVM)
* Implementiert in Java
* 100% Open Source (Apache 2.0) und kostenlos

----

## Plattformunabhängig

* Java-Ökosystem: Java, Groovy, Kotlin, Scala, ...
* Native Projekte: C, C++, Swift, ...
* Android
* Sonstiges: Go, Asciidoctor, ...

![Technologien](technologies.png)
<!-- .element class="plain" -->

----

## Gradle in Zahlen

* Mehr als 7M Downloads/Monat
* OSS-Projekt #17 weltweit
* 35+ Gradle Engineers
* 300K Builds/Woche @ LinkedIn

----

## Gradle Inc.

> Build Happiness

* Produkte: *Gradle Build Scans* and *Gradle Enterprise*
* [Consulting, Support, Development Services etc.]
* [Training: online, öffentlich und in-house]

----

## Agenda

* Kurze Einführung in Gradle
* Inkrementelle Builds
* Gradle Plugins
* Build Scans
* Build Cache

---
<!-- .slide: data-background-color="#01303a" -->
# Kurze Einführung in Gradle

----

## Hello World

```gradle
tasks.register("helloWorld") { // in build.gradle
  doLast {
    println("Hello World!")
  }
}
```

```plain
$ gradle helloWorld

> Task :helloWorld
Hello World!

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

----

## Tasks

- Ein Gradle-Build führt einen/mehrere Tasks aus.
- Tasks können von anderen Tasks abhängen.
- Tasks haben Inputs und Outputs.

----

## Buildscripts

Ein Gradle-Projekt wird mittels Buildscripts konfiguriert:

- `settings.gradle[.kts]`: Konfiguriert, welche Subprojekte Teile des Builds sind.
- `build.gradle[.kts]`: Konfiguriert, Plugins und Tasks, die im Build verwendet werden.

----

## Groovy vs. Kotlin DSL

- Buildscript verwenden eine Domain-Specific-Language (DSL).
- Ursprünglich wurde dazu *Groovy* verwendet.
- Seit Gradle 5.0 gibt es eine stabile DSL in *Kotlin*.

----

## Gradle Wrapper

`./gradlew <tasks>` statt `gradle <tasks>`

- Ermöglicht Ausführung des Builds ohne vorherige Installation von Gradle: Lädt ggfs. die benötigte Version herunter.
- Entwickler und Build-Server verwenden immer diesselbe Version.

----

## Dateistruktur

```plain
$ gradle init --dsl=kotlin --type=java-application \
              --test-framework=junit --package=com.example \
              --project-name=new-project

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

```c
├── build.gradle.kts    // Buildscript
├── gradle/wrapper      // Wrapper JAR und Konfiguration
├── gradlew             // Wrapper script für Linux/macOS
├── gradlew.bat         // Wrapper script für Windows
├── settings.gradle.kts // Settings
└── src                 // Java source tree
    ├── main
    │   ├── java
    │   └── resources
    └── test
        ├── java
        └── resources
```

----

## settings.gradle.kts

```kotlin
rootProject.name = "new-project"
```

----

## build.gradle.kts

```kotlin
plugins {
    java // zum Kompilieren von Java-Quellcode
    application // zum Generieren von Startup-Scripts
}
repositories {
    jcenter() // zum Auffinden von Dependencies
}
dependencies {
    implementation("com.google.guava:guava:26.0-jre")
    testImplementation("junit:junit:4.12")
}
application { // Extension des 'application'-Plugins
    mainClassName = "com.example.App"
}
```

----

## Verfügbare Tasks?

```plain
$ ./gradlew tasks

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
...
```

---
<!-- .slide: data-background-color="#01303a" -->
# Gradle Plugins

----

## Plugins

```kotlin
plugins {
    java // zum Kompilieren von Java-Quellcode
    application // zum Generieren von Startup-Scripts
}
```

- Wenden andere Plugins an
- Definieren Tasks, z.B. `compileJava`
- Stellen Möglichkeiten zur Konfiguration bereit, z.B. die `application`-Extension

----

## Eigene Plugins

Ziel: Vereinheitlichung der Build-Logik über Projekt-Grenzen hinweg

```kotlin
plugins {
    id("com.example.myplugin") version "1.0"
}
```

```java
public class MyPlugin extends Plugin<Gradle> {
  @Override public void apply(Project project) {
    // ...
  }
}
```

---
<!-- .slide: data-background-color="#01303a" -->
# Inkrementelle Builds

----

## Developer Productivity

TODO

----

## Erster Build

```plain
$ ./gradlew --console=plain build
> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes
> Task :jar
[...]
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
> Task :test
> Task :check
> Task :build

BUILD SUCCESSFUL in 1s
7 actionable tasks: 7 executed
```

----

## Inkrementeller Build

```plain
$ ./gradlew --console=plain build
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar UP-TO-DATE
[...]
> Task :compileTestJava UP-TO-DATE
> Task :processTestResources NO-SOURCE
> Task :testClasses UP-TO-DATE
> Task :test UP-TO-DATE
> Task :check UP-TO-DATE
> Task :build UP-TO-DATE

BUILD SUCCESSFUL in 0s
7 actionable tasks: 7 up-to-date
```

---
<!-- .slide: data-background-color="#01303a" -->
# Build Scans

----

## Build Scan: Aktivierung

```plain
$ ./gradlew build --scan

BUILD SUCCESSFUL in 1s
7 actionable tasks: 5 executed, 2 up-to-date

Publishing build scan...
https://gradle.com/s/lu7dxy7quyoju
```

----

## Build Scan: Timeline

![Build Scan](build-scan-timeline.png)

----

## Build Scan: Dependencies

![Build Scan](build-scan-dependencies.png)

----

## Build Scans

- Beschleunigen die Lösung von Build-Problemen
- Einfach mit Kollegen teilbarer Link
- Kostenlose Nutzung auf [scans.gradle.com](https://scans.gradle.com/)
- Gradle Enterprise bietet zusätzliche Features:
  - Hosting auf eigenem Server
  - Vergleich zweier Build Scans
  - Entwicklung von Build-Metriken über die Zeit

---
<!-- .slide: data-background-color="#01303a" -->
# Build Cache

---

<!-- .slide: data-background="/assets/gradle-outro.jpg" -->
# Q & A
<!-- .element style="margin-top:4.8em" -->
