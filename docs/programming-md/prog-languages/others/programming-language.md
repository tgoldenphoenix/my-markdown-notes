# Programming Language Design

The concept of closure is mentioned in JavaScript, Python, Lua

first-class citizen, first class function

## Terminogolies

- Serialization (Object $\rightarrow$ JSON)
- Deserialization (JSON $\rightarrow$ Object)

## Programming Paradidigms

The main categories include imperative, declarative, object-oriented, and functional. Within these, there are sub-paradigms like procedural, logic, and event-driven programming

`Imperative Programming` (lập trình mệnh lệnh): This paradigm focuses on explicitly stating how a program should achieve its result by providing a sequence of commands that modify the program's state. Thuật ngữ này thường được dùng trái ngược với lập trình khai báo.  
It is simple, easy to understand  
Ngôn ngữ hướng kịch bản (scripting language) thuộc về imperative programming

**Declarative Programming** (lập trình khai báo): This paradigm focuses on what the program should compute rather than how it should compute it. The programmer specifies the desired outcome, and the system figures out the steps to achieve it

`Procedural Programming` (Procedure-Oriented Programming, lập trình hướng thủ tục, hướng hàm): A subset of imperative programming, where code is organized into procedures (functions or subroutines) that are called to execute specific tasks. Examples include C, Fortran, and Pascal

- `Functional Programming`:
  - The style focuses on writing and using functions instead of focusing on objects, as in OOP languages.
  - First-Class and Higher-Order Functions:
    * Pass a function as an argument to another function.
    * Return a function from a method.
    * Assign a function to a variable.
  - Pure function: a pure function always produces the same output for the same input and has no side effects (it doesn't change a global variable or print to the console).
  - Immutability: Instead of changing the contents of a list, you create a new list with the modified values. This makes code much safer for Multi-threading, as there is no risk of two threads changing the same data at once.
  - Example in java: Lambda expression, functional interface, stream API

`Object-Oriented Programming` (OOP, hướng đối tượng): This paradigm centers around the concept of "objects," which encapsulate data (attributes) and methods (functions) that operate on that data. OOP emphasizes concepts like encapsulation, inheritance, and polymorphism. Languages like Java, C++, Python, and Ruby are prominent examples

Event-driven (hướng sự kiện): Programs respond to user actions or system events. Click buttons, trigger event handler function.  
Giao diện website có buttons là event-driven programming

These paradigms are not mutually exclusive, and many modern languages support multiple paradigms, allowing developers to choose the most suitable approach for a given task. For example, Python and JavaScript are **multi-paradigm** languages (ngôn ngữ hỗ trợ nhiều mô hình lập trình khác nhau) that support both object-oriented and functional programming

## Comparing Languages

C and C++ remain popular for building operating systems and various other embedded systems because of their small runtime footprint and in spite of their lack of programming safety. This lack of safety can lead to programs crashing unpredictably and exposing security holes for viruses and the like; indeed, type-safe languages such as Java and C# have supplanted C and C++ in various applications when the additional runtime footprint is acceptable.

## Async

fetching data is asynchronous operation => [but why?](https://stackoverflow.com/questions/39170556/why-are-these-fetch-methods-asynchronous)

When you request data from a resource using the fetch API, you have to wait for the resource to finish downloading before you can use it. This should be reasonably obvious. JavaScript uses asynchronous APIs to handle this behavior so that the work involved doesn't block other scripts, and—more importantly—the UI.

When the resource has finished downloading, **the data might be enormous**. There's nothing that prevents you from requesting a monolithic JSON object that exceeds 50MB.

What do you think would happen if you attempted to parse 50MB of JSON synchronously? It would block other scripts, and—more importantly—the UI.

## Naming Convention

- `camelCase`
- `PascalCase`
- `snake_case`
- `kebab-case`
- `SCREAMING_SNAKE_CASE`

---

javascript & node.js

- variable with single underscore (`_private`) is to define **private variable**.
- Double underscore (`__`) is not of any convention in node.js. There were only two variables (called global objects) with double underscores in node.js.
  1. `__dirname` : used when to get the name of the directory that the currently executing script resides in.
  2. `__filename`: used to get the filename of the code being executed.

Có một cái library `Underscore.js` có syntax double underscore `__`. Nó được dùng trước khi ES6+. Hiện giờ trong older code bases có thể gặp nó. Sau này thì có `Lodash` với syntax only one underscore `_`.

---

Python

In Python, variable names with double underscores on both sides (like `__init__` or `__dict__`) are referred to as `Dunder` (Double Under) methods or Magic Methods.  
You should never create your own names using this dunder format. This is reserved for "core" internal Python functionality. A future version of Python might introduce a built-in feature with that same name, causing your code to break.

- The single leading underscore (`_var`) means `protected` attribute/methods.
- Doule leading underscore (`__var`) means `private` attribute/methods.
- `__init__` Denotes special built-in methods recognized by the language (e.g., constructor, iterator).
- A single underscore (`_`) is used as a variable name when you need to unpack a value but intend to ignore or throw away that value.

Trong nodeJs cũng có kiểu naming convention tương tự.