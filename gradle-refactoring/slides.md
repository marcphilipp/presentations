---
title: Gradle
theme: white
highlightTheme: atom-one-light
css: /assets/gradle-accento.css
revealOptions:
    transition: zoom
    slideNumber: false
    controls: false
    markdown:
        smartypants: true
        breaks: true
---

<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-title.png" data-background-size="cover" data-transition="fade" -->

# Customizing and Refactoring<br>Gradle Builds
<!-- .element style="font-size:160%; text-align:right; margin-top:-300px" -->

### Marc Philipp, Gradle Inc.
<!-- .element style="text-align:right; font-weight: 300" -->

----

## ![Marc Philipp](/assets/marc-philipp.jpg) <!-- .element style="width:25%; float:right;" --> Marc Philipp

*Software Engineer bei Gradle, Inc.*

JUnit ![5](/assets/junit5-logo.svg)<!-- .element class="plain" style="height:1em; margin:0; vertical-align:-.3ex" --> team lead


*Twitter:* [@marcphilipp](https://twitter.com/marcphilipp)
*Web:* [marcphilipp.de](https://www.marcphilipp.de)

----
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->
# What is Gradle?

----

## What is Gradle?

> Gradle is an open-source build automation tool

* based on the Java Virtual Machine (JVM)
* implemented in Java
* focused on flexibility and performance
* 100% open-source (Apache 2.0) and free

----

## Versatile

* Java ecosystem: Java, Groovy, Kotlin, Scala, ...
* Official build tool for Android
* Native projects: C, C++, Swift, ...
* And more: Go, Asciidoctor, ...

![Technologies](technologies.png)
<!-- .element class="plain" -->

----

## Gradle Inc.

* Vision: *Build Happiness*
* Mission: *Accelerate Developer Productivity*
* Products:
  - *Gradle Build Tool*
  - *Gradle Enterprise*
* more than 60 employees including over 40 engineers

----

## Agenda

> Quick & Dirty

* Basic concepts
* How to make a mess with ...
  * dependency management
  * custom tasks
  * custom configuration

----

# ✋

## Show of Hands

---
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->
# Basic Concepts

----

## Tasks

- a Gradle build executes tasks
- tasks can depend on other tasks
- tasks have inputs and outputs

----

## Hello World

```gradle
tasks.register("helloWorld") { // in build.gradle
  doLast {
    println("Hello World!")
  }
}
```

```
$ gradle helloWorld

> Task :helloWorld
Hello World!

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

----

## Build Scripts

A Gradle project is configured in build scripts:

- `settings.gradle[.kts]`: configures the subprojects that comprise the build
- `build.gradle[.kts]`: configures the used plugins and tasks

----

## Gradle Wrapper

`./gradlew <tasks>` instead of `gradle <tasks>`

- allows executing builds with prior installation of Gradle
- downloads required version unless already cached locally
- developers and CI server always use the same version

----

## Anatomy of a Gradle project

```plain
$ gradle init --dsl=kotlin --type=java-application \
              --test-framework=junit --package=com.example \
              --project-name=new-project

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

```c
├── build.gradle.kts    // build script
├── gradle/wrapper      // wrapper jar and configuration
├── gradlew             // wrapper script for Linux/macOS
├── gradlew.bat         // wrapper script for Windows
├── settings.gradle.kts // settings
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

include("subproject-a")
include("subproject-b")
```

----

## build.gradle.kts

```kotlin
plugins {
    java // to compile Java sources
    application // to generate startup scripts
}
repositories {
    jcenter() // to resolve dependencies
}
dependencies {
    implementation("com.google.guava:guava:26.0-jre")
    testImplementation("junit:junit:4.12")
}
application { // extension of the 'application' plugin
    mainClassName = "com.example.App"
}
```

----

## Groovy vs. Kotlin DSL

- build scripts use a Domain Specific Language (DSL)
- initially Gradle only supported *Groovy*
  - dynamically typed
  - limited IDE support
- *Kotlin DSL* is stable since Gradle 5.0

> Build scripts should be declarative -- complex logic does not belong here.

----

## Incremental Builds

- only execute tasks that are affected by changes in between two _subsequent_ builds
  - _inputs_ have changed
  - _outputs_ are present and unchanged
  - _task implementation_ has changed (e.g. different plugin version)
- keep outputs of all tasks that are _up-to-date_

----

## First Build

```plain
$ ./gradlew --console=plain build
> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes
> Task :jar
[...]
> Task :compileTestJava
> Task :testClasses
> Task :test
> Task :check
> Task :build

BUILD SUCCESSFUL in 5s
7 actionable tasks: 7 executed
```

----

## Subsequent Build

```plain
$ ./gradlew --console=plain build
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar UP-TO-DATE
[...]
> Task :compileTestJava UP-TO-DATE
> Task :testClasses UP-TO-DATE
> Task :test UP-TO-DATE
> Task :check UP-TO-DATE
> Task :build UP-TO-DATE

BUILD SUCCESSFUL in 0s
7 actionable tasks: 7 up-to-date
```

----

## Build Scans

- Accelerate debugging of build problems
- Private but shareable link
- Free to use on [scans.gradle.com](https://scans.gradle.com/)

```plain
$ ./gradlew build --scan

BUILD SUCCESSFUL in 1s
7 actionable tasks: 5 executed, 2 up-to-date

Publishing build scan...
https://gradle.com/s/lu7dxy7quyoju
```

› https://gradle.com/s/lu7dxy7quyoju

----

## Build Cache

- allows reusing task outputs of _any_ previous build
- local and remote cache

```plain
$ git pull
[...]
185 files changed, 4320 insertions(+), 1755 deletions(-)
```
<!-- .element class="fragment" -->

```plain
$ ./gradlew --build-cache sanityCheck

BUILD SUCCESSFUL in 1m 11s
1338 actionable tasks: 238 executed, 1100 from cache
```
<!-- .element class="fragment" -->

---
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->
# Dependency Management

----

## Demo

----

## Recap

- Don't duplicate dependency version
- Don't "inherit" dependencies from parent project
- Use `buildSrc` to collect dependency versions
- Use a `java-platform` to streamline dependency management

----

## More on Dependency Management

Free webinars:

- https://gradle.com/blog/dependency-management-fundamentals/
- https://gradle.com/blog/dependency-management-part-2-handling-conflicts/

---
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->
# Custom Tasks

----

## Demo

----

## Recap

- Don't define complex tasks directly in the build script
- Define them in the `buildSrc` project
- Allows for testing and reuse in subprojects

---
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->
# Custom Configuration

----

## Demo

----

## Recap

- Don't duplicate configuration logic in build scripts
- Think about what a proper DSL should look like
- Extract your custom logic into a script plugin
- Better: Extract your custom logic into a pre-compiled script plugin in `buildSrc`
- Next step: Extract it into a separate plugin to use it in independent projects

---
<!-- .slide: data-background="#01313a" data-background-image="/assets/gradle-subsection.png" data-background-size="cover"-->

![](/assets/gradle-thank-you.png)
<!-- .element class="plain" style="width:30%" -->

## Thank you!

[@marcphilipp](https://twitter.com/marcphilipp)
