# JUnit & Mockito Testing

JUnit is a framework that enables and supports automated testing in Java.

## Terminologies

A lifecycle method is a method that is annotated with `@BeforeAll, @AfterAll, @BeforeEach`, or `@AfterEach`.

## JUnit Basics

Unit cần test trong Java là 1 class or 1 method inside class.

Move cursor inside the class, hit `Cmd S t` to create unit test

`JUnit` is a **testing framework** that provides the foundation for writing and running unit tests. It offers annotations (like `@Test, @BeforeEach, @AfterEach`) to define test methods and their setup/teardown, and assertion methods (like assertEquals, assertTrue) to verify expected outcomes. JUnit focuses on structuring your tests and validating the behavior of your code.

`JUnit 5` runs test cases using assertions, annotations, and test runners. It focuses mainly on methods and classes.

Mockito provides methods to create mock objects, configure their behavior (what they return), and verify certain interactions that took place (if the method was called, how many times, with what type of parameter, etc.).

Test functions always return `void`.

- BDD: Behaviour-driven development
- AAA: Arrange - Act - Assert

Instead of a Runner, JUnit 5 is built on the `JUnit Platform`, which uses **Test Engines** to discover and execute tests.

**Jupiter & Vintage** là 2 cái test engine.

JUnit 5 (JUnit Jupiter) does **not** use the concept of a "Runner" class like JUnit 4 did.

`@Test`: Indicates that a method is a test case.

The JUnit class `Parameterized` is one of JUnit’s many test runners. A test runner allows you to tell JUnit how a test should be run.

JUnit creates a new instance of the test class before invoking each `@Test` method. You cannot reuse instance variable values across test methods.

## JUnit Architecture

JUnit 5 was designed to work with old JUnit 4 code in existing legacy projects through JUnit Vintage.

JUnit 4, released in 2006, has a simple, monolithic architecture. All of its functionality is concentrated inside a single jar file. If a programmer wants to use JUnit 4 in a project, all they need to do is add that jar file on the classpath.

---

A JUnit 4 `runner` is a class that extends the JUnit 4 abstract Runner class. A JUnit 4 runner is responsible for running JUnit tests.

`Extensions` are the JUnit 5 equivalent of JUnit 4 runners.

---

A JUnit 4 rule is a component that intercepts test method calls; it allows you to do something before a test method is run and something else after a test method has run. Rules are specific to JUnit 4.

By using runners and rules, you can extend the monolithic architecture of JUnit 4. You may still encounter a lot of JUnit 4 code that uses runners and rules; plus, migrating to the equivalent JUnit 5 mechanisms (extensions) is not straightforward. So you may keep runners and rules in your code for a while, even if you move your work to JUnit 5.

---

JUnit 5 breaks the single JUnit 4 jar file into several smaller files.

- The JUnit 5 architecture contains three modules (figure 3.7):
  - JUnit Platform serves as a foundation for launching testing frameworks on the Java virtual machine (JVM). It also provides an API to launch tests from the console, IDEs, and build tools.
  - `JUnit Jupiter` combines the new programming and extension model for writing tests and extensions in JUnit 5. The name comes from the fifth planet of our solar system, which is also the largest.
  - `JUnit Vintage` is a test engine for running JUnit 3- and JUnit 4-based tests on the platform, ensuring backward compatibility.

`JUnit Jupiter` provides the test engine.

## Parameterized Tests

**Parameterized Tests**: run a test many times with different sets of parameters.

`@CsvSource({ “1, One”, “2, Two” })`: Supplies multiple test cases in CSV format.

## Annotations

`@BeforeEach` Denotes that the annotated method should be executed before each `@Test`

Setup object thì dùng `private`, thêm `final` nếu cần.

## Assertions

Kỹ thuật `import static` trong java.

The `fail()` method does not return an exception. Instead, it throws an `AssertionError`.  
In Java, `AssertionError` is not an `Exception`. Instead, it is a subclass of `Error`. Both Error and Exception are completely separate branches under the top-level `Throwable` class.  
Because `AssertionError` is an Error and not an Exception, the catch (Exception e) block will NOT catch it.

## Mockito

Mockito is a mocking framework used to create "mock" objects or "test doubles." These mocks simulate the behavior of real dependencies (e.g., other classes, external services, databases) that your code under test interacts with. Mockito allows you to define how these mock objects should behave when their methods are called, enabling you to isolate the specific unit of code being tested and control its environment. This is crucial for achieving true unit testing, where you test a single component in isolation without relying on its complex or external dependencies.

Mockito creates `mock objects` that mimic real dependencies. These objects can return predefined responses and track method calls.

By replacing dependencies with mock objects, we can prevent tests from depending on databases, APIs, or external systems. This ensures tests run in isolation, making them faster and more reliable.

`Stubbing` lets you define method responses for mock objects, ensuring predictable behavior in tests regardless of external factors. This allows precise control over test conditions and expected outcomes.

**Test-Driven Development (TDD)** is a software development approach where tests are written before the actual code.

```java
package com.springinaction.knights;
import static org.mockito.Mockito.*;
import org.junit.Test;
public class BraveKnightTest {
  @Test
  public void knightShouldEmbarkOnQuest() {
    Quest mockQuest = mock(Quest.class);
    BraveKnight knight = new BraveKnight(mockQuest);
    knight.embarkOnQuest();
    verify(mockQuest, times(1)).embark();
  }
}
```

`verify(mockQuest, times(1)).embark();` verifies that the method embark() was called exactly once on the mock object `mockQuest`.

You call `verify()` **after** you call `.embarkOnQuest()`

---

you can mock concrete classes and interfaces

### Why mocking?

Say you need to test `PokemonService.createPokemon()` which in turn call `PokemonRepository.save()`.

- You mock `PokemonRepository` but do not stub its `.save()` method: Mockito's default behavior for any unstubbed method that returns an object is to return `null`.
- You run an Integration Test without mocks. If you don't mock `pokemonRepository` and instead inject a real repository instance.
  - It executes real SQL: Spring/Hibernate will actually attempt to run an `INSERT` statement into your database.
  - If no database is running: The test will crash with a connection error.
  - If a database is running: It will insert into your actual database, causing test pollution (garbage test data in your DB).

The goal of a Unit Test for `PokemonService` is to test only the service's logic (e.g., mapping, validation, transformations)—not whether the repository work or not.

By mocking pokemonRepository:

- The test runs in milliseconds without needing a database connection.
- You isolate the test so that if `pokemonRepository` breaks, your PokemonService unit test doesn't fail for the wrong reason.

### What do I need to mock?

Do not mock types you don’t own: third-party libraries, framework classes, or external APIs—such as `HttpClient`, `AWS S3 Client`, `JDBC Connection`.

When you mock your own `PokemonRepository` you control both the mock and the implementation.

### Verifing Behaviour

Once created, a mock will remember all interactions. Then you can selectively verify whatever interactions you are interested in.

```java
 //mock creation
 List mockedList = mock(List.class);

 //using mock object
 mockedList.add("one");
 mockedList.clear();

 //verification
 verify(mockedList).add("one");
 verify(mockedList).clear();
```

---

Verifying exact number of invocations / at least x / never

```java
 //using mock
 mockedList.add("once");

 mockedList.add("twice");
 mockedList.add("twice");

 mockedList.add("three times");
 mockedList.add("three times");
 mockedList.add("three times");

 //following two verifications work exactly the same - times(1) is used by default
 verify(mockedList).add("once");
 verify(mockedList, times(1)).add("once");

 //exact number of invocations verification
 verify(mockedList, times(2)).add("twice");
 verify(mockedList, times(3)).add("three times");

 //verification using never(). never() is an alias to times(0)
 verify(mockedList, never()).add("never happened");

 //verification using atLeast()/atMost()
 verify(mockedList, atMostOnce()).add("once");
 verify(mockedList, atLeastOnce()).add("three times");
 verify(mockedList, atLeast(2)).add("three times");
 verify(mockedList, atMost(5)).add("three times");
```

`times(1)` is the default. Therefore using times(1) explicitly can be omitted.

### Stub

A `mock object` does not have a predetermined behavior. When running a test, you are setting the expectations on the mock before effectively using it. You can run different tests, and you can reinitialize a mock and set different expectations on it. The pattern of testing with a mock object this: initialize mock > set expectations > execute test > verify assertions.

When you `stubs` a method call, you provide a predetermined behavior right from the beginning. The stubbed code is written outside the test, and it will always have a fixed behavior, no matter how many times or where you use the stub; its methods will usually return hardcoded values. The pattern of testing with a stub this: initialize stub > execute test > verify assertions.

You create a mock object > you stub its methods with the syntax: `when({mock object call one of its methods}).thenReturn({the value that you want});`

---

```java
//You can mock concrete classes, not just interfaces
LinkedList mockedList = mock(LinkedList.class);

//stubbing
when(mockedList.get(0)).thenReturn("first");
when(mockedList.get(1)).thenThrow(new RuntimeException());

//following prints "first"
System.out.println(mockedList.get(0));

//following throws runtime exception
System.out.println(mockedList.get(1));

//following prints "null" because get(999) was not stubbed
System.out.println(mockedList.get(999));
```

- Once stubbed, the method will always return a stubbed value, regardless of how many times it is called.
- By default, for all methods that return a value, a mock will return either `null`, a primitive/primitive wrapper value, or an empty collection, as appropriate. For example 0 for an int/Integer and false for a boolean/Boolean.

### Argument matchers

```java
//stubbing using built-in anyInt() argument matcher
when(mockedList.get(anyInt())).thenReturn("element");

//stubbing using custom matcher (let's say isValid() returns your own matcher implementation):
when(mockedList.contains(argThat(isValid()))).thenReturn(true);

//following prints "element"
System.out.println(mockedList.get(999));

//you can also verify using an argument matcher
verify(mockedList).get(anyInt());

//argument matchers can also be written as Java 8 Lambdas
verify(mockedList).add(argThat(someString -> someString.length() > 5));
```

### Spy

`spy()` creates partial mocking, real methods are invoked but still can be verified and stubbed

## API Mock

Integration Test with an API stub (like WireMock)

## Temporary directory/file

k

## Rules

Junit 4

Note that if you’re using JUnit 5, rules have been replaced by the Extension model.

## Class, Suite & Runner

Test class (or TestCase or test case)—A class that contains one or more tests represented by methods annotated with `@Test`. Use a test class to group together tests that exercise common behaviors. In the remainder of this book, when we mention a **test**, we mean a method annotated with `@Test`; when we mention a **test case (or test class)**, we mean a class that holds these test methods—a set of tests. There’s usually a one-to-one mapping between a production class and a test class.

**Suite (or test suite)**—A group of tests. A test suite is a convenient way to group together test classes that are related. For example, if you don’t define a test suite for a test class, JUnit automatically provides a test suite that includes all tests found in the test class (more on that later). A suite usually groups test classes from the same package.

**Runner (or test runner)**—A runner of test suites. JUnit provides various runners to execute your tests. We cover these runners later in this chapter and show you how to write your own test runners.

On a daily basis, you need only write test classes and test suites. The other classes work behind the scenes to bring your tests to life.

To run a basic test class, you needn’t do anything special; JUnit uses a test runner on your behalf to manage the lifecycle of your test class, including creating the class, invoking tests, and gathering results.  
There are situations that may require you to set up your test to run in a special manner (invoking tests with different inputs).

## Assert methods

An assert method is silent when its proposition succeeds but throws an exception if the proposition fails.

- `assertEqual(double expected, double actual, double delta)`
  - Nếu compare `integer` thì pass `delta = 0`. Most often, the delta parameter can be zero, and we can safely ignore it. It comes into play with calculations that aren’t always precise, which includes many floating-point calculations. The delta provides a range factor. If the actual value is within the range expected - delta and expected + delta, the test will pass. You may find it use-ful when doing mathematical computations with rounding or truncating errors or when asserting a condition about the modification date of a file, because the precision of these dates depends on the operating system.
  - If the actual value isn’t equal to the expected value, JUnit throws an unchecked exception, which causes the test to fail.

The requirements to create a test method are that it must be annotated with @Test, be public, take no arguments, and return void.

JUnit creates a new instance of the test class before invoking each test method annotated with `@Test`. This helps provide independence between test methods and avoids unintentional side effects in the test code. Because each test method runs on a new test class instance, we can’t reuse instance variable values across test methods.

Assert methods with two value parameters follow a pattern worth memorizing: the first parameter (A in the table) is the expected value, and the second parameter (B in the table) is the actual value.

It’s a best practice to provide an error message for all your assert method calls.

- `assertArrayEquals("message", A, B)`
- `assertEquals("message", A, B)`: Asserts the equality of objects A and B. This assert invokes the equals() method on the first object against the second.
- `assertSame("message", A, B)`: Asserts that the A and B objects are the same object. Whereas the previous assert method checks to see that A and B have the same value (using the equals method), the assertSame method checks to see if the A and B objects are one and the same object (using the `==` operator).
- `assertTrue("message", A)`: Asserts that the A condition is true
- `assertNotNull("message", A)`: Asserts that the A object isn’t null.

