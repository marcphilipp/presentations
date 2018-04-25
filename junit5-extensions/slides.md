---
title: JUnit 5 Extensions
theme: white
highlightTheme: atom-one-light
css: /assets/junit5.css
revealOptions:
    transition: zoom
    slideNumber: false
    controls: false
    markdown:
        smartypants: true
        breaks: true
---

# JUnit 5 Extensions

----

## ![Marc Philipp](/assets/marc-philipp.jpg) <!-- .element style="width:30%; float:right;" --> Marc Philipp

*Software Engineer at ![Gradle](/assets/gradle.svg) <!-- .element class="plain" style="vertical-align: -35px; height:2em;" -->*

JUnit Maintainer since 2012

*Twitter:* [@marcphilipp](https://twitter.com/marcphilipp)
*Web:* [marcphilipp.de](https://www.marcphilipp.de)

---

## 🎉<br>JUnit 5 is here!

**5.0**
September 10, 2017

**5.1**
February 18, 2018

**5.2**
April 29, 2018

----

![JUnit 5 Architecture](junit5-architecture-big-picture.svg)
<!-- .element class="plain" style="width:100%" -->

----

## JUnit Jupiter

![JUnit Jupiter Modules](junit5-jupiter-modules.svg)
<!-- .element class="plain" style="width:50%" -->

----

## JUnit Jupiter API

- programming model for test authors
- extension model for extension authors

----

## Programming model

```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class SimpleTest {
    @Test
    @DisplayName("1 + 1 = 2")
    @ExtendWith(MyCustomExtension.class)
    void onePlusOneEqualsTwo() {
        assertEquals(2, 1 + 1);
    }
}
```

----

## Extension Model

```java
import org.junit.jupiter.api.extension.*;

class MyCustomExtension
        implements BeforeEachCallback, AfterEachCallback {
    @Override
    public void beforeEach(ExtensionContext context) {
        // setup
    }
    @Override
    public void afterEach(ExtensionContext context) {
        // teardown
    }
}
```

----

## Extension Registration

- *Declarative:* Annotate your test classes or methods with `@ExtendWith` to register extensions
    - May be used as a meta-annotation
- *Global:* Opt-in support for registration via ServiceLoader

---

# Conditional Test Execution

----

## Demo

----

## Extension Context

* Represents the current node in the test tree, e.g. test method or class
* Provides access to meta information about such a node, e.g. display name, method, class

----

## Extension Context

![Extension Context](extension-context.svg)
<!-- .element class="plain" style="width:100%;" -->

----

## Lessons Learned

* How to implement custom logic to determine whether a test class/method should be skipped?
* How to use *global* extension registration?
* How to deactivate a condition without changing the code using system properties?

---

# Reusable Test Setup & Teardown

----

## Demo

----

## Why the Store abstraction?

* Methods within an extension need to save and retrieve data, e.g. to cleanup in the end
* `Extensions` are instantiated once and called for multiple tests
* Using instance variables would be error-prone

----

## Store

* `Map`-like interface for extensions to save and retrieve data, e.g. `store.put(key, value)`
* Accessed via a `Namespace`: Enables sharing data across extensions, but makes it a deliberate decision (e.g. `Namespace.GLOBAL`)
* Reading from a `Store` follows the hierarchy upwards, if a key is not found

----

## Store

![Store](store.svg)
<!-- .element class="plain" style="width:100%;" -->

----

## Lessons Learned

* Using the `Store` class for extension state
* Using Lifecycle Callbacks to enable reuse of common setup/teardown code
* Implementing multiple `Extension` interfaces in a single extension

---

# Resolving Test Parameters

----

## Demo

----

## Lessons Learned

* How to resolve test method parameters in an `Extension`?
* You can also inject parameters into test class constructors and `@BeforeEach`/`@AfterEach` methods

---

# Providing Arguments for Parameterized Tests

----

## Demo

----

## Lessons Learned

* How to use Parameterized Tests?
* Writing a custom `ArgumentsProvider` that loads data from a JSON file, ...

---

# From Tests to Test Templates

----

## Demo

----

## Lessons Learned

* How to execute a test multiple times with different contexts?
* How to implement a `TestInvocationContextProvider`

----

## Support Classes

Package `org.junit.platform.commons.support` contains:

* `AnnotationSupport` to scan for custom annotations, including meta-annotations
* `ReflectionSupport` for classpath scanning, finding methods, invoking them etc.

---

# Summary

----

## JUnit Jupiter is extensible

- A lot of extension points to choose from
- The JUnit team will add more in future releases
- Combining multiple extension points in one extension is very powerful!

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

## Example code

https://github.com/marcphilipp/junit5-extensions-demo

----

# Questions?

&nbsp;

👉 [@marcphilipp](https://twitter.com/marcphilipp) or [@junitteam](https://twitter.com/junitteam)

----

# Thanks!
