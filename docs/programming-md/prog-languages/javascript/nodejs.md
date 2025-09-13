# Nodejs notes

## Overview

Nodejs a programming language that’s almost—but not quite—entirely like JavaScript. It is used on the server-side.

npm is a tool that manages packages written in Nodejs JavaScript.

The V8 engine is written in C++, it compile Javascript code into machine code.

## Asynchronous Programming

Node.js uses an event-driven, non-blocking model that allows it to handle many operations concurrently without waiting for previous tasks to complete. This is particularly useful when building **I/O-intensive applications**, such as web servers, where operations like reading files, accessing databases, or making HTTP requests can be done in parallel.

Spring (Java) Asynchronous Model: Multi-threaded, Thread-based Asynchronous Execution: Spring applications typically run on a Java Virtual Machine (JVM) and leverage its multi-threading capabilities. Asynchronous operations are achieved by executing tasks in separate threads, often managed by **thread pools**.

### Concurrency Vs Parallelism

concurrent connections: existing or happening at the same time

Concurrency:

- One CPU core, multiple tasks

Parallelism:

- Multiple CPU cores

## The V8 Engine

Node.js combines the power of Google’s open-source V8 JavaScript engine, an event-driven architecture, and a robust event loop to deliver high performance and scalability.

V8 is written in C++. It is used in Google Chrome (web browser) and in Node.js, among others.

V8 compiles JavaScript into machine code rather than interpreting it, resulting in significantly faster execution.

Key aspects of V8 include:

1. Just-In-Time (JIT) Compilation: V8 uses a JIT compiler to optimize performance by converting JavaScript code into machine code at runtime.
2. Garbage Collection: V8 includes a garbage collector that automatically reclaims memory no longer in use, improving memory management and efficiency.
3. Hidden classes: are dynamically created internal structures that V8 uses to optimize property lookups and reduce the overhead of object access.
4. Inline caching

## Concurrency Model & The Event Loop

Javascript (or Node) is single threaded runtime V8, which means one thread == one callstack == one thing at a time. The callstack of the V8 engine can only do one thing at a time. Nếu nó gặp 1 long runing task thì program freeze. Unlike multi-threaded languages where tasks can run in parallel, JavaScript must manage multiple tasks on this single thread.  
While the javascript engine runs on a single thread (đơn luồng) both on the browser and nodejs, the **event loop & Concurrency mode** allows it to handle multiple tasks happening at the same time.

In the **V8 runtime** (or engine), we essentially have two things:

1. **The heap** is for memory allocation of reference types (object, array, function)
2. **The callstack** is for managing execution contexts of function and storage of primitive value (number, string, boolean)

Btw, khi read call-stack error thì cái ở dưới cùng sẽ gọi từ từ lên cao.

The **browser runtime** is more than just the V8 engine. Inside the browser, we have V8 plus:

- Web APIs (or background APIs in Node.js): DOM methods & properties, ajax, setTimeout, `fetch`, localStorage, sessionStorage I/O tasks. These APIs handle the operation separately without blocking the main thread. Nodejs dùng C++ APIs
- event loop: essential for non-blocking concurrency model
- **callback queue** (task queue, macrotask queue) and Microtask Queue
- Node.js runtime không có Web APIs nhưng có event loop & callback queue

Asynchronous code is executed after a task that runs in the “background” finishes (set timer, load image, geolocation API, ajax calls)

- Với cách hoạt động của callstack, nếu gặp task slow thì nó sẽ stop and wait until it finish and get pop off the stack (free nếu gặp long-running tasks). Nên nó sẽ tìm cách offload these task qua cho webapi with a callback nếu có thể.
- Khi `setTimeout()` được add to the call stack, nó đẩy qua webapi with the callback, the WebAPIS (which provide us with `setTimeout()` now, on its own thread, set a timer with a callback (**in its own thread** not the V8 single thread).
- The `setTimeout()` pop off the stack, the code move on
- The timer set by the WebAPI is now complete, but the webapis can't just chuck stuff onto the Call Stack (it would appear randomly in the middle of your code). This is where the **task queue or callback queue** kicks in.
- When the timer is done, the webapi pushes the callback of the `setTimeout()` onto the task queue.
- The job of the **event loop** is to looks at the Call Stack and the task queue. Only when the Call Stack is **empty/clear**, it then take the first thing on the queue and put it on the stack which effectively run it.

The Web APIs (in the browser) or background APIs (in Node.js), with its own thread:

- set timer for `setTimeout`
- fetch data from api
- Process any time consuming task (fetch, I/O tasks)

callback fuction are also stored in the webapis. When triggered, callbacks are pushed to the task queue and the event loop push it to the call stack. Khi offload long-running tasks to the webapis thì có 2 cách:

1. dùng callback (callback-based api)
2. Dùng promise (promise-based apis)

So the V8 runtime is single thread, but the browser has other thread from its APIS.

In Nodejs, the I/O operations (read/write to file, process images) is passed off to the system kernel, which handles it asynchronously. Node.js only executes the callback function once the task is complete, allowing other tasks to be processed in the meantime.

`setTimeout` is not the guaranteed time to run, it is the minimum time to run.

### Microtask queue

The microtask queue is dedicated to:

- Promises' resolved value.
- A function body execution after `await`

The Event Loop prioritizes the microtask queue over the Callback queue. Nếu **Call Stack empty** thì nó ưu tiên task chờ trong microtask queue hơn Task Queue để đẩy vào call stack.

- Khi mình gọi `fetch()` webapi, nó tạo promise object trong webapi's thread
- Khi promise resolve thì `.then(callback)` được push vào Microtask queue ngồi chờ

Macrotask sẽ chỉ được thực hiện sau khi Callstack đã trống và Microtask queue trống

1 Microtask có thể trigger 1 microtask khác, và dẫn tới microtask queue không bao giờ trống và cứ thế chạy mãi có thể gây giật lag hoặc thậm chí đơ trình duyêt. Nhưng với Macrotask thì không thế, vì mỗi lần Macrotask thực hiện xong nó sẽ trả control về cho Event loop. Hay nói cách khác thì mỗi macrotask được thực hiện trên 1 iteration của Event loop (dạng như 1 lần lặp)

- với Worker thì nó sẽ được chạy trên 1 thread khác, và có event loop của riêng nó. Thường khi ta nói Main Thread (hoặc UI Thread) ý chỉ tới cái thread chính dùng để làm việc với UI, hầu hết là code của ta được thực thi ở thread này, Main thread mà bị block thì web của chúng ta sẽ bị treo và Chrome sẽ báo lỗi Page unresponsive
- chú ý rằng với mỗi thread, ta chỉ có 1 Event loop: ví dụ Main thread có 1 event loop, mỗi Web worker có 1 event loop riêng. Và khi ta mở mỗi tab trình duyệt thì sẽ là 1 môi trường riêng biệt có Event loop riêng
- Tất cả các Microtasks phải được thực hiện xong trước khi trình duyệt xử lý event handling/rendering hoặc trước khi thực hiện Microtask tiếp theo

Chốt lại, về cơ bản ta có thứ tự thực hiện như sau: Code đồng bộ > Microtask > Macrotask (`setTimeout, setInterval`)

### Execution context

Function Execution Context (FEC):

- A new FEC is created whenever a function is invoked.
- Each FEC has its own scope, containing variables and functions defined within that particular function.
- When a function completes its execution, its FEC is typically removed from the call stack.

## FAQs

CommonJS vs ESM

[How to update nodejs and npm](https://nodejs.org/en/download/package-manager)

## Commands

Show version `node -v`

Distinguish `npm init` vs `npm install`

## Terminologies

npm: Node Package Manager || nvm: Node Version Manager [Distinguish npm vs nvm](https://stackoverflow.com/questions/32660993/difference-between-npm-and-nvm)

## References

[What the Heck Does “npm” Mean?](https://css-tricks.com/a-clear-definition-of-npm-and-what-it-does/) by CSS Tricks

```bash
```bash
export NVM_DIR="$HOME/.config/nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

dd
