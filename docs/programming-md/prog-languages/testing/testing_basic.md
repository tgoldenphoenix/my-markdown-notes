# Software Testing & Diagraming Notes

## Terminologies

Sequence diagram: Sơ đồ tuần tự

Testing: kiểm thử phần mềm

Tester: kiểm thử viên

`Functional testing` simply means evaluating the compliance of a system or component with the requirements.

PRD: Product Requirements Document

## Manual Testing vs Automation Testing

Automation testing là phương pháp kiểm thử tự động. Người tester sẽ phải viết các kịch bản kiểm thử sau đó sử dụng các tool hỗ trợ để thực hiện kiểm thử, phương pháp này sẽ giúp việc kiểm thử hiệu quả và tốn ít thời gian hơn. Automation testing giúp chạy các kịch bản kiểm thử lặp lại nhiều lần và các task kiểm thử khác khó thực hiện bằng tay như performance testing và stress testing.

**Unit tests** are a type of automated testing that focuses on verifying the functionality of individual units or components of code, typically functions or methods, in isolation.

- Theo mức độ từ nhỏ lên cao; from the innermost (narrowest) to the outermost (broadest):
  - Unit test
  - Integration test
  - Smoke testing
  - Regression testing
  - System test
  - Customer/User Acceptance test

## Designing Test case

- `Black-box Testing (kiểm thử hộp đen)`: Focuses on functionality without knowing the internal code. Có 5 kỹ thuật sau đây:
  1. **Equivalence Partitioning (phân vùng tương đương)**: Divides input data into valid and invalid partitions.
  2. **Boundary Value Analysis (phân tích giá trị biên)**: Tests edge values of input fields.
  3. **Decision Table (Bảng quyết định)**: Maps input combinations to expected outcomes.
  4. **State Transition (chuyển đổi trạng thái)**: Evaluates behavior based on state changes.
  5. Error Guessing (đoán lỗi)
- `White-box Testing`: Focuses on internal logic and code structure. Techniques include:
  - `Statement Coverage (bao phủ câu lệnh, C0)`: Ensures all code statements are executed.
  - `Branch Coverage (bao phủ nhánh, C1)`: Tests decision points (if-else conditions).
  - Path Coverage (bao phủ đường dẫn): Verifies all possible paths through the code.

- Gọi là black-box test vì tester không thấy phần bên trong. Công việc cần làm là nhập dữ liệu đầu vào (input) và kiểm tra kết quả trả về có đúng như mong muốn hay không.
- White-box test vì tester thấy phần code bên trong. Phải viết code.

## SRS - Software Requirements Specification

Người Nhật gọi SRS là `user story`.

Agile/scrum chỉ dùng user story.

## Control Flow Path

Đồ thị luồng điều khiển. Liên quan tới white-box.

- `If` hình diamond
- `A = B` dùng Rectangle
- Không bắt buộc dùng hình nào cả.

## Unit Test

You should use a separate testing database for your tests. Do not pollute your production database with testing data.  
Tests will add and manipulate all kinds of data, which can lead to data being lost or to the database being in an inconsistent state.  
Using a separate database also makes it easier to determine a bug’s root cause. Because you are fully in control of the test database’s state, customers’ actions won’t interfere with your tests’ results.

Automated tests also don’t eliminate the need for manual testing. Verifying your work as end users would do and investing time into **exploratory testing** are still indispensable. Because this book is targeted at software developers instead of QA analysts, in the context of this chapter, I’ll refer to the unnecessary manual testing process often done during development just as manual testing.

A **unit test** examines the behavior of a distinct unit of work. Within a Java application, the “distinct unit of work” is often (but not always) a single method. A unit of work is a task that isn’t directly dependent on the completion of any other task.  
In contrast, `integration tests` and `acceptance tests` examine how various components interact.

Here’s a generic description of a typical unit test from our perspective: “Confirm that the method accepts the expected range of input and that the method returns the expected value for each input.”

## The Testing Pyramid

k

## Class Diagram

k

