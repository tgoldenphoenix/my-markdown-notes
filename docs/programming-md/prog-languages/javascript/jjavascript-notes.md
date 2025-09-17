# Vanilla Javascript notes

## Basics

There’re many categories of operators: mathematical operators, comparison op, logical op, assignment op, etc.

+=, -=, `/=`, `\*=` are also valid  
x--; (2 dấu minus) is valid too.

We usually link JS file at the end of the `<body>` tag, not in the `<head>` tag like CSS stylesheet.

`for...in` use on object to iterates over the keys  
`for...of` use on array to iterates over items.

## Data types

Javascript has 7 primitive data types. These data types are immutable, meaning their values cannot be changed once created. They are:

1. String
2. Number: không có sự phân biệt giữa `float` vs `int`, only `number`
3. `Boolean`
4. `BigInt`
5. `Null`: Represents the intentional absence of any object value.
6. `Undefined`: Represents a variable that has been declared but not yet assigned a value.

Java has the concept of `null`, but it does not have undefined like JavaScript.  
JS is very dynamic in variable use, never knowing wtf you are defining. Therefore, `undefined` exists to differentiate between explicit nulls and actual missing values

In Javascript, all values are **truthy** except `false, 0, -0, 0n, empty string, null, undefined, NaN`

In Java:

- `null` is a special literal value that represents the absence of a reference to an object.
- `null` is not an object or a type itself, but it can be assigned to reference variables of any type.

## ES6

Từ ES6 (2015, 9 years ago) về sau gọi là Modern JavaScript. React uses ES6.

Introduce the `class` keyword, constructor.  
Inheritance using `extends` keyword. Introduce `super()` method.

Other features introduced in ES6 (2015):

- Default parameters
- Arrow functions expression
- `let` and `const` and block-scoped. Trước đó xài `var`.
- Many array methods such as: `.map()`
- Destructuring
- Spread Operator; Rest parameter and defaults `...`
- Ternary Operator
- Template Strings literal, multi-line string
- Iterators and the for-of loop
- Proxy

The scope is global when a var variable is declared outside a function.

### Spread & Rest operators

Using the spread operator to overwrite an object property [here](https://dev.to/dailydevtips1/javascript-overwrite-property-in-an-object-3g32)

### ES6 Module and the CommonJS specification

Javascript was created in 1995. NodeJS was created in 2009.

There was no built-in module system in the early days of JavaScript. Codes were written in a global scope, rendering functions and variables accessible globally, resulting in naming conflicts and complex codebases. The lack of encapsulation and modularity made it difficult for developers to reuse code across multiple projects.  
The evolution of JavaScript modules has resulted in a more organized and maintainable approach to writing code, allowing for the effective encapsulation and management of code dependencies.

Node.js uses the **CommonJS** module system (CJS) to structure code into reusable components. By default, Node.js treats JavaScript code as CommonJS modules.

- CJS is synchronous and good for back end. But CJS will not work in the browser. It will have to be transpiled and bundled. CSJ ra đời trước khi ES6 tồn tại nên một số old nodejs codebase still use CSJ.
- Syntax of CSJ: `require('express')`, `module.exports`
- Node.js comes with a set of built-in modules, and additional third-party modules can be installed via npm (Node’s package manager).
- CSJ is synchronous loading because you can be sure that the file will be there on the server
- `module` là 1 global variale; `module.exports` is an object.
- The `require()` function import a module (another nodejs file)
- When you import a module using require(), you invoke it. (exporting wrapped in a function)

AMD stands for **Asynchronous Module Definition**. AMD is made for frontend (when it was proposed) (while CJS backend). It is asynchronous loading which improved page load time and responsiveness.

Từ ES6 có ES6 Module (ECMAScript modules). Dùng `import`, `export` keywords.

There are 02 ways to enable ES modules in a Node.js:

1. changing the file extensions from `.js` to .mjs.
2. Add a `type: module` field inside `package.json`. With that inclusion, Node.js treats all files inside that package as ES modules, and you won’t have to change the file to a .mjs extension.

Every module can have two different types of export, **named export and default export**. You can have multiple named exports per module but **only one** default export. Hoặc không có export default only named export.

When importing non-defaults, you must specify the exact same name (optionally renaming it with `as`), but the default export can be imported with any name.  
If you want to import non-default value, you must use destructuring syntax `{...}`. Import default value do not use curly braces.

## Variables & Scoping

Data types are categorized into `Primitive` and `Reference` (tham chiếu) types in JavaScript. In JS, every value is either an object or a primitive value.

Also note that values have type, but the variable storing that value don’t have type. Variables simply store values that has a type. In JS, mình phải distinguish value vs variable, variable is the box that store values!

Primitive types are stored in the call stack.
Reference data are stored inside the **heap**. When reference data is created, the variable is placed on the call stack but the actual value is placed on the heap. For every object, a pointer is added to the **stack**, and this pointer points to the object on the heap.

There are 3 ways to declare variables in JS. `let` and `const` were introduced in ES6, so they’re modern JavaScript. `var` is the old way of declaring.  
`let` is used to declared variables that can be re-assigned later  
`const` is used to declare variables that are not meant to be re-assigned.

You cannot declare an empty variable (undefined) using `const`.

As the best practice for writing clean code, You should use const by default and use let only when you’re sure that the variable needs to change at some point in the future. It’s a good practice to have as little mutations or (variable changes) as possible. Changing variables has the potential to cause bugs.

`null` vs `undefined`

- `undefined` means a variable has been declared but has not yet been assigned a value
- `null` is an assignment value. It can be assigned to a variable as a representation of no value

### Immutability

If a data type is **mutable**, that means that you can change it. Mutability allows you to modify existing values without creating new ones.  
Immutable: cannot be changed or added to. Can only be _re-assigned_. All primitive values in JS are immutable.

Primitive data types (string included in JS) are _immutable_.
By default, **reference data types** are mutable. In JS, reference data types consist of _Functions, Arrays, and Objects_.

```javascript
let age = 22;
age = 30; // re-assign not mutate

const person = {
  name: "anhao",
  age: 23
}
person.age = 25; // mutate
```

With `const`, we cannot re-assign, but we can mutate object & array delcared with `const`.

In JS, distinguish **type conversion** vs type coercion:

- type conversion: when we manually convert data types.
- type coercion: JavaScript automatically converts types behind the scene for us.

**Function declarations** are hoisted, which means they are loaded into memory at compilation. The function call works even before the function declaration appears.  
Anonymous functions, on the other hand, are not hoisted.

### Scoping

- local scope (function scope): define inside a function
- global variable: define outside any function or block
- block Scope (ES6): inside `{}` (`if(){}`); accessible only inside block; only applies to `const` and `let` variables.

Undeclared variables (created without a keyword `var, let, const`), are always global, even if they are created inside a function.

The `const` declaration declares block-scoped local variables. The value of a constant can't be changed through reassignment using the assignment operator, but if a constant is an object, its properties can be added, updated, or removed.

A `const` variable also cannot be uninitialized. This code would throw an error:

```javascript
const firstName;
```

Declare Array, Objects thì luôn dùng `const`. Vì không muốn change the type. Primitive values cũng ưu tiên `const` hơn `let`.

The `let` declaration declares re-assignable, block-scoped local variables, optionally initializing each to a value.

You should completely avoid using `var`. Just know how it works for legacy reasons. `var` is pretty much like let, you can mutate the variable later on your code. But they’re actually pretty different below the surface. Just don’t use var anymore.

`let` and `const` varibles are not hoisted.

**Lexical scope** is the definition area of an expression.

### Closure

Closure is when a function "remembers" the values in its environment at the time of created, even after those functions have finished running.  
More formally (but still in simple terms), a closure is created when a function retains access to variables from its outer scope, even after that scope has finished executing.

Closures are essential for creating functions that **maintain its state**, without relying on global variables.  
Global variable thì open to everyone. With closures, the scope (visibility) of the variables can be better controlled, which means the possible unintended side effects can be better controlled.

Some design patterns that utilize closure:

- Creating private variables
- Managing asynchronous code
- Building function factories
- currying and memoization.

[bài blog rất hay](https://www.trevorlasn.com/blog/understanding-javascript-closures) của trevorlasn ví dụ tạo một counter function có thể increment, decrement, reset its state using closure.

## String manipulation

Just like in Python, using `f-string` is much easier than casting + concatenate. For this, JS also has something called **Template literals**. Template literal uses back tick (\`) and $\{} as placeholders. In Holy C thì place holder là `%s, %f, %i`.  
This Template literal is actually an ES6 feature, imagine before ES6 :v

`String.split()` trả về 1 array, input một **separator** (`\`)

## Flow control

In the normal for loop, we must use `let`: `for (let i = 0; i < count; i++) {}`  
But in `for...of`, we can use `const` because the variable only exists for a single iteration, not during the entire loop.

The `for...of` loop dùng cho arrays, string. It iterates over elements of array.  
The `for...in` loop dùng cho objects. It iterates over keys in object.

```JavaScript
const array = ["a", "b", "c"];
for (const element of array) {
  console.log(element);
}

const object = { a: 1, b: 2, c: 3 };
for (const property in object) {
  console.log(`${property}: ${object[property]}`);
}
```

**for loops** are best used when you know the number of iterations ahead of time, whereas a **while loop** is best used when you don't know the number of iterations in advance

In JavaScript, a **callback** is a function that is passed as an argument to another function, with the intention that it will be executed later, typically after the completion of an asynchronous operation or a specific event.

## Array

- `pop()` remove & return last item. `push()` adds to end of array return new length.
- `shift()` remove & return the first item. `unshift()` adds to beginning of arr, return new length.

- does not change the original array: `map(), reduce()`
- mutate original array: `sort(), pop(), push(), unshift()`

- `map()` creates a new array by applying a function to all elements of the original array. Không bị mất đi element nào hết.
- The `filter()` returns a new array containing elements that passed a specified test condition. Sẽ ít hơn array ban đầu.
The callback function should return true to keep the element, or false otherwise. In the callback, only the element is required.
- The `reduce()` method reduces an array of values down to **just one value**. To get the output value, it runs a reducer function on each element of the array.

- `.indexOf` based on value
- `.findIndex` based on test condition

### Slice & Splice

The `slice()` method returns selected elements in an array, as a new array. It does not change the original array.

When passing the range to `slice()`, the 1st value is inclusive and the 2nd value (the end value of the range) is exclusive.  
If pass only one argument to `slice()` => slice off, not slice in a range.

The `splice()` method adds and/or removes array elements. It overwrites the original array.
In `splice()`, the second argument is not the end of the range. Rather, It is the delete counter value.

Use `toSplice()` return a new array without mutating the original array.

### indexOf() & findIndex()

`indexOf()` find by compare equal. Returns `-1` if not present.

findIndex() find by testing function. Can compare greater (>) or less (<).

## Object in JS

There are two kinds of object properties:

1. The first kind is **data properties**. We already know how to work with them. All properties that we’ve been using until now were data properties.
2. **accessor property** are essentially **functions** that execute on getting and setting a value, but look like **regular properties** to an external code.

Trong JS còn có **map**:

- Use when you need keys that are NOT strings
- Better performance than normal key-value pair objects

## Prototypes, inheritance

In JavaScript, objects have a special hidden property `[[Prototype]]` (as named in the specification), that is either `null` or references another object (other types are ignored). That object is called “a prototype”  
We use `__proto__` property to access `[[Prototype]]` (a historical getter/setter). The value of `__proto__` can be either an object or null. Other types are ignored. There can be only one `[[Prototype]]`. An object may not inherit from two others.

In Javascript, we say that `animal` (cha) is the prototype of `rabbit` (con) or `rabbit` **prototypically inherits** from animal”.

- Java uses a class-based model where Class tạo ra instances.
- JavaScript uses a prototype-based model. Object được tạo ra từ constructor functions. Every object has a prototype, which is another object. When a property or method is accessed on an object, JavaScript first checks if it exists on the object itself. If not, it looks in its prototype, and so on up the prototype chain.

"Constructor" trong JS is just a normal function with the `this` keyword inside. We can use the `new` keyword to create objects from this constructor function.
The ES6 `class` keyword is just syntactic sugar. Under the hood, it still use prototype and **constructor functions**, old ES5 syntax. ES6 classes do **NOT** behave like classes in “classical OOP”.  
Chỉ có `Object.create()` là works differently.

Constructor function in Java phải nằm trong một class.

JavaScript has built-in constructors for all native objects:

```javascript
new Object()   // A new Object object
new Array()    // A new Array object
new Map()      // A new Map object
new Set()      // A new Set object
new Date()     // A new Date object
```

- Use object literals `{}` instead of `new Object()`.
- Use array literals `[]` instead of `new Array()`.
- Use pattern literals `/()/` instead of `new RegExp()`.
- Use function expressions `() {}` instead of `new Function()`.

```javascript
 "";           // primitive string
0;            // primitive number
false;        // primitive boolean

{};           // object object
[];           // array object
/()/          // regexp object
function(){}; // function
```

Strings are primitive in JS. You can call method on them like they are object is because JS auto add a **wrapper**.

[window](https://developer.mozilla.org/en-US/docs/Web/API/Window) is the DOM object. When calling methods inside `window`, do not need to specify namespace.

[console](https://developer.mozilla.org/en-US/docs/Web/API/console) is also an object.

- The object referenced by `[[Prototype]]` is called a “prototype”.
- If we want to read a property of or call a method, and it doesn’t exist, then JavaScript tries to find it in the prototype.
- Chỉ khi **read** properties or methods mà tìm trong child object không có thì JS mới tìm up the prototype chain. Còn **write/delete** thì sẽ thực hiện directly on the object, không tìm tới prototype object.

The keyword `this` is not affected by prototypes at all. **No matter where the method is found: in an object or its prototype. In a method call, `this` is always the object before the dot.**  
So, the setter call `admin.fullName`= uses `admin` as `this`, not `user`.

Every function has the `prototype` property even if we don’t supply it. If `F.prototype` is an object, then the `new` operator uses it to set `[[Prototype]]` for the new object.

`Person.prototype` is not the prototype of the `Person()` constructor functions. It is what's gonna be used as the prototype for all objects created using the `Person` constructor function. It's pretty fucked up. I know!  
The `new` keyword auto link the newly created object (`anhao.__proto__`) to `Person.prototype`.

`Person.prototype` object properties & methods được dùng chung (share among object), created object do NOT own them. Tránh tạo duplicate, giảm memory.

`Person.prototype.constructor === Person()`

### ES6 class syntax

Remember: **real class do not exists in Javascript**

Trong JS, có thể tạo object mà không cần dùng class.

In JS, a class is similar to functions.

constructor function trong JS phải đặt tên đúng `constructor()`, không trùng với Class name.

**Static methods** in JS is owned by the constructor function NOT the prototype. You can only call `Array.from()` not `[1,2,3].from`. All the arrays do NOT inherit this static method.

### The `this` and `super` keyword

`this` is not affected by prototypes at all. No matter where the method is found: in an object or its prototype. In a method call, `this` is always the object before the dot. As a result, methods are shared, but the object state is not.

The `this` keyword do NOT work with arrow functions. In this case, `this` will reference the `window` object.  
Phải gọi `super()` trước khi muốn dùng `this` keyword khi `extends` in JS.

Inside **Event listener**, `this` refers to the DOM element that the handler is attached to

Read more:

- [Traversy Media vid](https://www.youtube.com/watch?v=vDJpGenyHaA)

## DOM manipulation

`const el = document.getElementById("idd")` rồi `console.log(el)` thì sẽ ra `[object HTMLDataElement]`. Đó là lí do phải dùng những thuộc tính đặc biệt sau đây để access & modify nội dung mình cần.

- `document.getElementById("id-name").innerHTML` seletcs the contents between a tag's beginning and end including its children HTML tags but excluding the parent HTML tags.
- `element.outerHTML` = inner + the parent's HTML tags

Use innerHTML as default. This replaces only the content (if using i.e. "=") inside the current element referred to. If you are using outerHTML, then the element referred to will also be replaced.

- Use innerHTML when you're setting text inside of an HTML tag like an anchor tag, paragraph tag, span, div, or textarea.
- Use `appendChild()` method If you're trying to add new DOM elements inside of another DOM element: append an item to a list, Move an item from one list to another

- `element.innerText` returns the text as it is rendered on screen. It ignores all HTML tags and also elemenents that are hidden with CSS styles. Đôi khi selected content sẽ giống với `.innerHTML`.
- `element.textContent` reads text as it is in the markup (HTML), including hidden text by CSS. It still ignore HTML tags. It does NOT consider CSS styles

Đối với những tags có `value=""` attribute thì `element.value` sẽ select cái đó. Nếu `<select>` drop-down list có nhiều child elements thì only select cái parent element  
Đối với tags không có `value=""` attribute thì `element.attributes[0].value` sẽ lấy value of any attribute you want. Còn `el.attributes` là một `[object NamedNodeMap]`.

## Error Handling

Usually, a script “dies” (immediately stops) in case of an error, printing it to console.  
But there’s a syntax construct `try...catch` that allows us to “catch” errors so the script can, instead of dying, do something more reasonable such as:

- Show friendly error message to front-end users with toastify; suggest an alternative to the visitor
- Help programmers in debugging
- Record error to log files
- Automatically notify dev with email
- Send a new network request

If an error occurs, then the `try` execution is stopped, and control flows to the beginning of `catch (err)`. The `err` variable (we can use any name for it) will contain an error object with details about what happened. We can just ignore this `err` object and just have a simple `console.log()` or we can use it to perform advance error handling techniques.  
In this way, an error inside the `try {...}` block does not kill the script – we have a chance to handle it in catch.

The `err` object has two important properties: `name` and `message`.

`try...catch` only works for runtime errors (also called **exceptions**) which is when the code can still complied and there is no syntax errors. Vì vậy 2 khái niệm exceptions và `try-catch` luôn đi đôi với nhau.

`try...catch` works synchronously. If an exception happens in “scheduled” code, like in setTimeout, then try...catch won’t catch it. To catch an exception inside a scheduled function, try...catch must be inside that function.

### What if you don't catch it?

If you have a runtime error do you best to resolve it, don't try to measure how bad it is or rely on current situation that maybe works now, but may not work tomorrow. A runtime error means there's a bug in the script. You need to fix it.

In JavaScript running in the browser, whether an uncaught runtime exception "freezes" the screen or still allows rendering depends on **where the error happens** and **what part of the browser loop it interrupts**.

The UI thread (main thread) is responsible for **both JavaScript execution and screen rendering**. If JavaScript code throws an error and **keeps the thread blocked**, rendering cannot happen → screen appears frozen. If the error is thrown but the event loop can recover and continue, the browser will still repaint/render.

```javascript
button.onclick = () => {
  throw new Error("oops");
};
// The handler crashes, but the browser logs the error and continues processing future events and rendering.

setTimeout(() => {
  throw new Error("oops later");
}, 1000);
// The error bubbles up, shows in console, but rendering continues.
```

- Blocking errors stop the event loop → no rendering.
- Thrown errors just kill the current callback, but the event loop continues → rendering goes on.
- Screen freezes if your code prevents the browser’s event loop from continuing.
- Screen keeps rendering if the error is thrown in a contained callback or async task—the loop can still proceed.

---

- **Node.js** is single-threaded (with an event loop).
- All incoming HTTP requests share the same main thread.
- Async I/O (database calls, file system, HTTP requests, etc.) is handled via the event loop + libuv threadpool.
- But your JavaScript code still runs on the single thread.

```javascript
app.get("/user/:id", (req, res) => {
  throw new Error("oops"); // sync error
});
```

- This will crash the request handler.
- If you don’t have error-handling middleware, the **whole server process may crash** depending on configuration.
- Unlike Spring, there’s no separate thread per request — so one bad throw can terminate everything.

```javascript
app.get("/user/:id", async (req, res) => {
  const user = await db.findUser(req.params.id); 
  if (!user) throw new Error("User not found"); // inside async
  res.send(user);
});
```

- Without a .catch() or Express error middleware, this rejected promise becomes an unhandled rejection.
- Depending on Node.js version:
  - Older: logged to console, request hangs.
  - Newer: can crash the process.
- This is why async/await + try/catch is so common in Node.js.

```javascript
app.get("/block", (req, res) => {
  while (true) {} // blocks event loop
});
```

- This freezes the entire server.
- No other requests can be handled until the loop ends.
- This is the Node.js equivalent of “freezing the UI” in a browser.

- Spring Boot (Java): one request per thread → error affects only that request.
- Node.js (Express): single-threaded → error handling must be explicit, or it can crash/freeze the entire server.

That’s why:

- Node.js relies so much on async/await, .catch(), error middleware.
- In production, people often use process managers (PM2, Docker restart policies) to auto-restart a crashed Node process.

### Throwing our own error

Nếu không có runtime error (exception) nhưng logic bị lỗi (to us programmer only, not to the code) we’ll use the `throw` operator.

Technically, we can use anything as an error object. That may be even a primitive, like a number or a string, but it’s better to use objects, preferably with name and message properties (to stay somewhat compatible with built-in errors).

JavaScript has many built-in constructors for standard errors: `Error`, `SyntaxError`, ReferenceError, TypeError and others. We can use them to create error objects as well.  
For built-in errors (not for any objects, just for errors), the `name` property is exactly the name of the constructor. And `message` is taken from the argument.

### Rethrowing technique

The rule is simple: Catch should only process errors that it knows and “rethrow” all others.

Trong `catch{}` block, if we don’t know how to handle the error, we do `throw err`. This re-thrown error will “falls out” of its `try...catch` and can be either caught by an outer `try...catch` construct (if it exists), or it kills the script.

You can NOT ignore exceptions, you either handle them or they will eventually terminate the script.

## Callback, Promises and Async await

Synchronous là "đồng bộ", Async là "bất đồng bộ"

A function that does something asynchronously should provide a `callback` argument where we put the function to run after it’s complete (timer for example).

AJAX: Asynchronous JavaScript And XML: Allows us to communicate with remote web servers in an asynchronous way. With AJAX calls, we can request data from web servers dynamically.  
It uses `XMLHttpRequest` (XHR) objects to interact with servers. Hiện nay người ta dùng JSON.

Nesting multiple callbacks (more than 3) is not a good idea. In which case, we use `Promises`  
A `promise` is a special JavaScript object that links the **producing code** and the “consuming code” together. The “producing code” takes whatever time it needs to produce the promised result, and the “promise” makes that result available to all of the subscribed code when it’s ready.

```javascript
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```

The **executor function** passed to `new Promises()` has two arguments: `resolve` and `reject`. They are callbacks provided by the JavaScript engine itself.

- You can call `reject()` with a string error message. But it is recommended to use `Error` objects (or objects that inherit from Error).
- You can call them without any parameter
- The executor function do NOT run when we defined it with `new Promise()`, it only run when we call `promise.then()` on the promise object.

- `resolve("done!")` will return a string `"done"` and become the input for `.then`
- `reject( new Error("Whoops!") )` returns an error object as the input for `.catch`

The `new Promise` constructor return a **promise object** that has two internal properties:

- `state` — initially `pending`, then changes to either `fulfilled` when `resolve(value)` is called or `rejected` when `reject(error)` is called.
- `result` — initially `undefined`, then changes to `value` when `resolve(value)` is called or `error` when reject(error) is called. This will become the input of `.then, .catch & await`
- A promise that is either resolved or rejected is called `settled`, as opposed to an initially `pending` promise.

A Promise object serves as a link between the executor (the “producing code” or “singer”) and the consuming functions (the “fans”), which will receive the result or error. **Consuming functions** (promise handlers) can be registered (subscribed) using the methods `.then` and `.catch` (don't confused with the `try...catch` construct. We say that we "add handlers" (subscribing functions) to the promise object using `.then()`.

- `promise.then()` can takes in two functions as arguments. The first function will runs when the promise is `resolved` and receives the `result` as parameter. The second function runs when the promise is rejected and receices the error.
- `.then(null, errorHandlingFunction)` is the same as `.catch(errorHandlingFunction)`. In order words, the call `.catch(f)` is a complete analog of `.then(null, f)`, it’s just a shorthand.
- If a promise is `pending` (it has asynchronous codes), `.then/catch/finally` handlers will wait for its outcome.
- Sometimes, it might be that a promise is already settled when we add a handler to it. In such case, these handlers just run immediately

Just like there’s a finally clause in a regular `try {...} catch {...}`, there’s `.finally()` in promises. The idea of finally is to set up a handler for performing cleanup/finalizing after the previous operations are complete. E.g. stopping loading indicators, closing no longer needed connections, etc.

- A `finally` handler has no arguments. In finally we don’t know whether the promise is successful or not. That’s all right, as our task is usually to perform “general” finalizing procedures.
- A finally handler also shouldn’t return anything

- `try {...} catch {...} finally {...}`
- `promise.then().catch().finally()`

We can add many `.then` on a single promise. Each time, we’re adding a new “fan”, a new subscribing function, to the “subscription list”. These different `.then` each process the promies independently. This is different from **promise chaining**, a technique where we chain multiple `.then` consecutively. In practice we rarely need multiple handlers for one promise. Chaining is used much more often.

- Promise chaining works because every call to a `.then(handler)` always returns a new promise with a new `resolve(result)`, so that we can call the next `.then` on it just like we attach .then to a normal promise.
- When a handler returns a value, it becomes the (new) fulfilled result of that promise, so the next `.then` is called with it (synchronously).
- A handler can also create and return a promise like [this](https://javascript.info/promise-chaining#returning-promises). Returning promises like this allows us to build chains of asynchronous actions. This is a useful technique. While if `.then` only return a value, it is only synchronous.

Returning promises in `.then` allows us to build chains of asynchronous actions.
A normal `return` statement in `.then` only return a new resolved result and is synchronous.

As a good practice, an **asynchronous action should always return a promise**. That makes it possible to plan actions after it; even if we don’t plan to extend the chain now, we may need it later.

- Nếu dùng `Promise` thì dùng `.catch()` để catch errors
- Còn nếu dùng `async await` thì phải dùng `try catch` block để catch error.

- When a promise reject, code jump to `.catch()`
- The code of a promise executor and promise handlers has an “invisible try..catch” around it. If an exception happens, it gets caught and treated as a rejection.

### Async/await

The word `async` before a function means one simple thing: **an async function always returns a promise**. Other values are automatically wrapped in a resolved promise.

- When you declare a function with the `async` keyword, it implicitly returns a promise.
- If the function returns a non-promise value, JavaScript automatically wraps that value in a resolved promise like `Promise.resolve(1)`.
- If the async function throws an error, it returns a rejected promise.

The keyword `await` only works in `async` function. The keyword makes JavaScript wait until that promise settles and returns its result (either `resolve('result')` or `reject(error object)`). That doesn’t cost any CPU resources, because the JavaScript engine can do other jobs in the meantime: execute other scripts, handle events, etc.

`await` is just a more elegant syntax of getting the promise result than `promise.then`. And, it’s easier to read and write.

When a method returns a Promise object, we can `await` that returned promise object.

If a promise resolves normally, then `await` promise returns the result. But in the case of a rejection, it throws the error, just as if there were a throw statement at that line. We can catch that error using `try..catch`, the same way as a regular throw.

When we use async/await, we rarely need `.then`, because await handles the waiting for us. And we can use a regular `try..catch` instead of .catch. That’s usually (but not always) more convenient.

You can `await` a promise to be fulfilled like [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#awaiting_a_promise_to_be_fulfilled)

`await` makes JavaScript wait, but it doesn’t Block the Event Loop.  
When await is used inside an async function, The function execution is paused at the await statement. This pause only affects the function where await is used, not the entire JavaScript execution.  
If the awaited operation takes time (e.g., an API call), it runs independently, while the event loop continues processing other tasks.

### Compare with Async in Java

Java and JavaScript approach asynchronous programming with different underlying mechanisms, reflecting their distinct execution models.

JavaScript is single-threaded and uses an event loop to handle asynchronous operations. This means it achieves concurrency without true parallelism

Promises are the JS syntax for asynchronous processing -- which is mandatory in large parts of JS for historical reasons. It started life as a language meant to run in the main thread of a single-process browser, where waiting more than a fraction of a second for anything would cause the browser to noticeably freeze up or lag. Languages that don't have that sort of history (Java) aren't like this, and are synchronous by default.

reddit [What's the point of JavaScript promises?](https://www.reddit.com/r/learnjavascript/comments/w4qram/whats_the_point_of_javascript_promises/)

Node.js model:

- Single-threaded event loop: Node.js runs most user code in **one thread**.
- If you perform a blocking operation (like reading from DB, waiting for HTTP, or disk I/O), the entire thread pauses → no other requests can be handled.
- To avoid blocking, Node.js APIs are `asynchronous` and return Promises.
- `async/await` is just syntax sugar for working with these Promises in a clean, readable way.
- This keeps the event loop free to handle other requests.

Spring Boot / Java model:

- Multi-threaded by default: Spring Boot runs in a traditional thread-per-request model.
- Each HTTP request gets its own worker thread from a thread pool.
- If one thread is waiting for a DB call, other requests can still be handled by other threads.
- Blocking I/O is acceptable here because you’re not stuck with only one thread.
- No `await` needed, because the blocking happens on its own thread, not the main one.

```javascript
// Node.js
const user = await db.findUserById(1); // non-blocking
// Spring Boot
User user = userRepository.findById(1); // blocking call
```

- Node.js must avoid blocking at all costs → async/await everywhere.
- Spring Boot can block safely, because it has a thread pool to handle multiple requests concurrently.

But wait — Java can be async too. Spring Boot also supports **reactive programming** (e.g., Spring WebFlux with Project Reactor). This is more like Node.js, but Java developers usually stick with the blocking model unless they need **massive scalability**.

- Node.js = single-threaded → needs async/await to avoid blocking.
- Spring Boot = multi-threaded → can use blocking I/O without hurting performance (to some extent).
- Async style exists in both, but Node.js makes it mandatory, while Spring Boot makes it optional.

## Chrome Dev Tool

hard reload `Ctrl F5`

## FAQs & terms

faq

## References

[javascript.info](https://javascript.info/js)
