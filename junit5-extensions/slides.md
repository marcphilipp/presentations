---
title: JUnit 5 Extensions
theme: white
highlightTheme: darcula
css: junit5-extensions/junit5.css
revealOptions:
    slideNumber: true
    controls: false
    markdown:
        smartypants: true
        breaks: true
---

# JUnit 5 Extensions

---

# 🎉

## JUnit 5 is released!

Release date: *September 10, 2017*

----

![JUnit 5 Architecture](/assets/junit5-extensions/junit5-architecture-big-picture.svg)
<!-- .element class="plain" style="width:100%" -->

----

## JUnit Jupiter

![JUnit Jupiter Modules](/assets/junit5-extensions/junit5-jupiter-modules.svg)
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

## Lessons Learned

* How to implement custom logic to determine whether a test class/method should be skipped?
* How to use *global* extension registration?
* How to deactivate a condition without changing the code using system properties?

---

# Reusable Test Setup & Teardown

----

## Demo

----

## Lessons Learned

* Using Lifecycle Callbacks to enable reuse of common setup/teardown code
* Implementing multiple `Extension` interfaces in a single extension
* Using the `Store` class for extension state

---

# Resolving Test Parameters

----

## Demo

----

## Lessons Learned

* How to resolve test method parameters in an `Extension`?
* You can also inject parameters into test class constructors and `@BeforeEach`/`@AfterEach` methods

---

# Custom Sources for Parameterized Tests

----

## Demo

----

## Lessons Learned

* How to use Parameterized Tests?
* Writing a custom source that loads data from a JSON file, ...

---

# From Tests to Test Templates

----

## Demo

----

## Lessons Learned

* How to execute a test multiple times with different contexts?
* How to implement a `TestInvocationContextProvider`
* Using `AnnotationSupport` to scan for custom annotations

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
* Sample projects for Gradle and Maven:  
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
https://github.com/marcphilipp/junit5-extensions-demo
