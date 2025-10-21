# JUnit & Mockito testing

JUnit is a framework that enables and supports automated testing in Java.

## JUnit test framework

Unit cần test trong Java là 1 class or 1 method inside class

Move cursor inside the class, hit `Cmd S t` to create unit test

**JUnit** is a **testing framework** that provides the foundation for writing and running unit tests. It offers annotations (like @Test, @BeforeEach, @AfterEach) to define test methods and their setup/teardown, and assertion methods (like assertEquals, assertTrue) to verify expected outcomes. JUnit focuses on structuring your tests and validating the behavior of your code.

JUnit 5 runs test cases using assertions, annotations, and test runners. It focuses mainly on methods and classes.

Mockito provides methods to create mock objects, configure their behavior (what they return), and verify certain interactions that took place (if the method was called, how many times, with what type of parameter, etc.).

**Jupiter & Vintage** là 2 cái test engine.

Test functions always return `void`.

- BDD: Behaviour-driven development
- AAA: Arrange Act Assert

## Test Annotations

`@Test`: Indicates that a method is a test case.

## Parameterized Tests

`@CsvSource({ “1, One”, “2, Two” })`: Supplies multiple test cases in CSV format.

## Mockito

Mockito is a mocking framework used to create "mock" objects or "test doubles." These mocks simulate the behavior of real dependencies (e.g., other classes, external services, databases) that your code under test interacts with. Mockito allows you to define how these mock objects should behave when their methods are called, enabling you to isolate the specific unit of code being tested and control its environment. This is crucial for achieving true unit testing, where you test a single component in isolation without relying on its complex or external dependencies.

Mockito creates mock objects that mimic real dependencies. These objects can return predefined responses and track method calls.

By replacing dependencies with mock objects, we can prevent tests from depending on databases, APIs, or external systems. This ensures tests run in isolation, making them faster and more reliable.

**Stubbing** lets you define method responses for mock objects, ensuring predictable behavior in tests regardless of external factors. This allows precise control over test conditions and expected outcomes.

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

## Temporary directory/file

k
