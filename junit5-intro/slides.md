---
title: JUnit 5
theme: white
highlightTheme: atom-one-light
css: /assets/jax-junit5.css
revealOptions:
    slideNumber: false
    controls: false
    markdown:
        smartypants: true
        breaks: true
---

<!-- .slide: data-background="./jax-background.jpg" -->
# JUnit 5
<!-- .element style="color:white; margin-top:2.2em" -->

#### The new Testing Framework for Java<br>and Platform for the JVM
<!-- .element style="color:white" -->

----

## ![Marc Philipp](/assets/marc-philipp.jpg) <!-- .element style="width:30%; float:right;" --> Marc Philipp

*Software Engineer at ![Gradle](/assets/gradle.svg) <!-- .element class="plain" style="vertical-align: -35px; height:2em;" -->*

JUnit Maintainer since 2012

*Twitter:* [@marcphilipp](https://twitter.com/marcphilipp)
*Web:* [marcphilipp.de](https://www.marcphilipp.de)

----

## 🎉<br>JUnit 5 is here!

**5.0**
September 10, 2017

**5.1**
February 18, 2018

**5.2**
*April 29, 2018*

----

## Agenda

1. How to write tests and extensions using JUnit 5?
2. What is the JUnit Platform and why do we need it?
3. What's still to come and how to get started?

---

<!-- .slide: data-background="./jupiter.jpg" -->
# JUnit Jupiter
<!-- .element style="color:white; text-shadow:-1px 0 black, 0 1px black, 1px 0 black, 0 -1px black" -->

#### The new Testing Framework for Java
<!-- .element style="color:white; text-shadow:-1px 0 black, 0 1px black, 1px 0 black, 0 -1px black" -->

Image: NASA <!-- .element style="font-size: 10px; color:white" -->

----

## Jupiter? 🤔

> Is writing tests rocket science now?

Nope, it's just a new name so we can easily distinguish it from the old JUnit and the other parts of JUnit 5.
<!-- .element: class="fragment" -->

... and it's the fifth planet from the sun.
<!-- .element: class="fragment" -->

----

## Basics (Demo)

https://github.com/marcphilipp/junit5-demo/tree/20180425-jax

----

## Basics (Recap)

- `@Test` is still a thing, but in `org.junit.jupiter.api`
- `@BeforeAll`, `@BeforeEach`, `@AfterEach`, `@AfterAll` have new names
- `Assertions` look similar -- a few new ones like `assertThrows`, `assertAll`
- Custom `@DisplayNames` to ease camel-case fatigue
- `@TestInstance(PER_METHOD` or `PER_CLASS)`
- `@Tag` instead of `@Category`

----

## More ways to test (Demo)

https://github.com/marcphilipp/junit5-demo/tree/20180425-jax

----

## More ways to test (Recap)

- `@ParameterizedTest` with different `@Source` annotations
  - `@ValueSource`, `@EnumSource`, `@CsvSource`, `@CsvFileSource`, `@MethodSource`, `@ArgumentsSource(MyProvider.class)`, `@YourCustomSource`
- `@RepeatedTest` for flaky tests
- `@TestFactory` to produce _dynamic_ tests

----

## Extensions (Demo)

https://github.com/marcphilipp/junit5-demo/tree/20180425-jax

----

## Extensions (Recap)

- Registration (as many as you need):
  - Declarative: `@ExtendWith` on classes or methods
  - Programmatic: `@RegisterExtension` on fields
  - Global: Via `ServiceLoader` (see [User Guide](https://junit.org/junit5/docs/current/user-guide/#extensions-registration-automatic))
- Implementation:
  - `Extension` marker interface
  - one extension -- _n_ extension points/interfaces

----

## Extension Points

- Lifecycle: `BeforeAllCallback`, `BeforeEachCallback`, `BeforeTestExecutionCallback`, `TestExecutionExceptionHandler`, `AfterTestExecutionCallback`, `AfterEachCallback`, `AfterAllCallback`
- Other: `ExecutionCondition`, `TestInstancePostProcessor`, `ParameterResolver`, `TestTemplateInvocationContextProvider`

----

## Third-Party Extensions

Spring, Mockito, Docker, Wiremock, JPA, Selenium/WebDriver, DbUnit, Kafka, Jersey, GreenMail, S3Mock, Citrus Framework, XWiki, ...

<https://github.com/junit-team/junit5/wiki/Third-party-Extensions>

----

## Agenda

1. How to write tests and extensions using JUnit 5? ✅
2. What is the JUnit Platform and why do we need it?
3. What's still to come and how to get started?

---

<!-- .slide: data-background="./space-shuttle-launchpad.jpg" -->
# JUnit Platform
<!-- .element style="color:white; text-shadow:-1px 0 black, 0 1px black, 1px 0 black, 0 -1px black" -->

#### Platform for Testing on the JVM
<!-- .element style="color:white; text-shadow:-1px 0 black, 0 1px black, 1px 0 black, 0 -1px black" -->

Image: NASA <!-- .element style="font-size: 10px; color:white" -->

----

## JUnit as a Platform?

- JUnit has always been a platform
  - for IDEs and build tools
  - for other testing frameworks
- Closely coupled (internal APIs, reflection, serialization)

----

## Coupling

----

## Separation of Concerns

1. An API to write tests (Jupiter API)
1. Extensible mechanisms to discover and execute tests (Test Engine SPI)
1. An API for test execution by tools (Launcher API)

----

![JUnit 5 Architecture](junit5-architecture-big-picture.svg)
<!-- .element class="plain" style="width:100%" -->

----

## Third-party Engines

----

## Vintage Engine

----

## Build Tool Support

Demo?

----

## Console Launcher

?

----

## IDE Support

----

## Compatibility

Demo?

----

## Migration Path

Demo?

----

## Agenda

1. How to write tests and extensions using JUnit 5? ✅
2. What is the JUnit Platform and why do we need it? ✅
3. What's still to come and how to get started?

---

<!-- .slide: data-background="./andromeda-galaxy.jpg" -->
# Roadmap and Resources
<!-- .element style="color:white; text-shadow:-1px 0 black, 0 1px black, 1px 0 black, 0 -1px black" -->

Image: NASA <!-- .element style="font-size: 10px; color:white" -->

----

## Important Future Milestones

- Parallel test execution
- Release of donated provider for Apache Maven Surefire
- New reporting format to support new features
- Parameterized Classes
- Scenario Tests
- ...

----

## Getting Started

* User Guide:
  http://junit.org/junit5/docs/current/user-guide/
* Sample projects for Gradle, Maven, and Ant:
  https://github.com/junit-team/junit5-samples
* Javadoc:
  http://junit.org/junit5/docs/current/api/

----

## Wanted: Feedback!

* StackOverflow:
  http://stackoverflow.com/questions/tagged/junit5
* Code & Issues:
  https://github.com/junit-team/junit5/
* Twitter:
  https://twitter.com/junitteam

----

## Thank you!

Example code:
https://github.com/marcphilipp/junit5-demo

[@marcphilipp](https://twitter.com/marcphilipp)
