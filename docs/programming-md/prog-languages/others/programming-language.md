# Programming Language Design

The concept of closure is mentioned in JavaScript, Python, Lua

first-class citizen, first class function

In Python, **REPL** is an acronym for Read, Evaluate, Print, and Loop. Developers use REPL Python to communicate with the Python Interpreter.  
In contrast to running a Python file, you can input commands in the REPL and see the results displayed immediately. The Python REPL also lets you print out object and method help, a list of all the accessible methods, and much more.

## Programming paradidigms

The main categories include imperative, declarative, object-oriented, and functional. Within these, there are sub-paradigms like procedural, logic, and event-driven programming

**Imperative Programming** (lập trình mệnh lệnh): This paradigm focuses on explicitly stating how a program should achieve its result by providing a sequence of commands that modify the program's state. Thuật ngữ này thường được dùng trái ngược với lập trình khai báo.  
It is simple, easy to understand  
Ngôn ngữ hướng kịch bản (scripting language) thuộc về imperative programming

**Declarative Programming** (lập trình khai báo): This paradigm focuses on what the program should compute rather than how it should compute it. The programmer specifies the desired outcome, and the system figures out the steps to achieve it

**Procedural Programming** (Procedure-Oriented Programming, lập trình hướng thủ tục, hướng hàm): A subset of imperative programming, where code is organized into procedures (functions or subroutines) that are called to execute specific tasks. Examples include C, Fortran, and Pascal

**Functional Programming**: A declarative style that treats computation as the evaluation of mathematical functions and avoids changing program state. It relies heavily on functions, higher-order functions, and avoiding side effects. Examples include Haskell, Lisp, and Scala.

**Object-Oriented Programming** (OOP, hướng đối tượng): This paradigm centers around the concept of "objects," which encapsulate data (attributes) and methods (functions) that operate on that data. OOP emphasizes concepts like encapsulation, inheritance, and polymorphism. Languages like Java, C++, Python, and Ruby are prominent examples

Event-driven (hướng sự kiện): Programs respond to user actions or system events. Click buttons, trigger event handler function.  
Giao diện website có buttons là event-driven programming

These paradigms are not mutually exclusive, and many modern languages support multiple paradigms, allowing developers to choose the most suitable approach for a given task. For example, Python and JavaScript are **multi-paradigm** languages (ngôn ngữ hỗ trợ nhiều mô hình lập trình khác nhau) that support both object-oriented and functional programming

## Async

fetching data is asynchronous operation => [but why?](https://stackoverflow.com/questions/39170556/why-are-these-fetch-methods-asynchronous)

When you request data from a resource using the fetch API, you have to wait for the resource to finish downloading before you can use it. This should be reasonably obvious. JavaScript uses asynchronous APIs to handle this behavior so that the work involved doesn't block other scripts, and—more importantly—the UI.

When the resource has finished downloading, **the data might be enormous**. There's nothing that prevents you from requesting a monolithic JSON object that exceeds 50MB.

What do you think would happen if you attempted to parse 50MB of JSON synchronously? It would block other scripts, and—more importantly—the UI.
