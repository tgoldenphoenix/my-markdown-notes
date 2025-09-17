# JUnit & Mockito testing

JUnit is a framework that enables and supports automated testing in Java.

## Manual Testing vs Automation Testing

Automation testing là phương pháp kiểm thử tự động. Người tester sẽ phải viết các kịch bản kiểm thử sau đó sử dụng các tool hỗ trợ để thực hiện kiểm thử, phương pháp này sẽ giúp việc kiểm thử hiệu quả và tốn ít thời gian hơn. Automation testing giúp chạy các kịch bản kiểm thử lặp lại nhiều lần và các task kiểm thử khác khó thực hiện bằng tay như performance testing và stress testing.

**Unit tests** are a type of automated testing that focuses on verifying the functionality of individual units or components of code, typically functions or methods, in isolation.

Theo mức độ từ nhỏ lên cao

- Unit test
- Integration test
- Smoke testing
- Regression testing
- System test
- User Acceptance test

**JUnit** is a **testing framework** that provides the foundation for writing and running unit tests. It offers annotations (like @Test, @BeforeEach, @AfterEach) to define test methods and their setup/teardown, and assertion methods (like assertEquals, assertTrue) to verify expected outcomes. JUnit focuses on structuring your tests and validating the behavior of your code.

JUnit 5 runs test cases using assertions, annotations, and test runners. It focuses mainly on methods and classes.

Mockito provides methods to create mock objects, configure their behavior (what they return), and verify certain interactions that took place (if the method was called, how many times, with what type of parameter, etc.).

## Mockito

Mockito is a mocking framework used to create "mock" objects or "test doubles." These mocks simulate the behavior of real dependencies (e.g., other classes, external services, databases) that your code under test interacts with. Mockito allows you to define how these mock objects should behave when their methods are called, enabling you to isolate the specific unit of code being tested and control its environment. This is crucial for achieving true unit testing, where you test a single component in isolation without relying on its complex or external dependencies.

