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

## Unit test

A **unit test** examines the behavior of a distinct unit of work. Within a Java application, the “distinct unit of work” is often (but not always) a single method. By contrast, integration tests and acceptance tests examine how various components interact. A unit of work is a task that isn’t directly dependent on the completion of any other task.

Here’s a generic description of a typical unit test from our perspective: “Confirm that the method accepts the expected range of input and that the method returns the expected value for each input.”

## Assert methods

- `assertEqual(double expected, double actual, double delta)`
  * Nếu compare `integer` thì pass `delta = 0`. Most often, the delta parameter can be zero, and we can safely ignore it. It comes into play with calculations that aren’t always precise, which includes many floating-point calculations. The delta provides a range factor. If the actual value is within the range expected - delta and expected + delta, the test will pass. You may find it use-ful when doing mathematical computations with rounding or truncating errors or when asserting a condition about the modification date of a file, because the precision of these dates depends on the operating system.
  * If the actual value isn’t equal to the expected value, JUnit throws an unchecked exception, which causes the test to fail.

The requirements to create a test method are that it must be annotated with @Test, be public, take no arguments, and return void.

JUnit creates a new instance of the test class before invoking each @Test method. This helps provide independence between test methods and avoids unintentional side effects in the test code. Because each test method runs on a new test class instance, we can’t reuse instance variable values across test methods.

Assert methods with two value parameters follow a pattern worth memorizing: the first parameter (A in the table) is the expected value, and the second parameter (B in the table) is the actual value.

It’s a best practice to provide an error message for all your assert method calls.

- `assertArrayEquals("message", A, B)`
- `assertEquals("message", A, B)`: Asserts the equality of objects A and B. This assert invokes the equals() method on the first object against the second.
- `assertSame("message", A, B)`: Asserts that the A and B objects are the same object. Whereas the previous assert method checks to see that A and B have the same value (using the equals method), the assertSame method checks to see if the A and B objects are one and the same object (using the `==` operator).
- `assertTrue("message", A)`: Asserts that the A condition is true
- `assertNotNull("message", A)`: Asserts that the A object isn’t null.