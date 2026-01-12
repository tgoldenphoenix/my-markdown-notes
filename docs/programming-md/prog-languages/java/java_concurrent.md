# Java Concurrent Programming

## Basics & Jargon

Java has two, mostly separate concurrency APIs: the older API, which is usually called `block-structured concurrency` or `synchronization-based concurrency` or even “classic concurrency,” and the newer API, which is normally referred to by its Java package name, `java.util.concurrent`.

The classic approach to concurrency was the only API available until Java 5. This is the language-level API that is built into the platform and depends upon the `synchronized` and `volatile` keywords.

- Concurrent programming is fundamentally about performance.
- There are basically no good reasons for implementing a concurrent algorithm if the system you are running on has sufficient performance that a serial algorithm will work.
- Modern computer systems have multiple processing cores—even mobile phones have two or four cores today.
- All Java programs are multithreaded, even those that have only a single application thread.

---

`Concurrent software` are applications that can perform multiple tasks simultaneously.

In concurrent programming, there are two basic units of execution: `processes` and `threads`. In the Java programming language, concurrent programming is mostly concerned with threads.

A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.

Most implementations of the `Java virtual machine` run as **a single process**. Multiprocess applications are beyond the scope of this lesson.

`Threads` are sometimes called lightweight processes. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.  
Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.

The main thread has the ability to create additional threads.

---

race condition in java is at the operating level while race condition in sql is different

The fundamental concept is the same (two things trying to change one thing at the same time)

In Java, you are managing Memory (RAM). In SQL, you are managing Data Persistence (Disk/Rows).

In java, multiple Threads within the same program try to access a shared variable, object, or memory address in RAM

- recurrent (adj): that happens again and again
- concurrent (with something): (adj) existing or happening at the same time

`mutual exclusion`: only one process or thread accesses a shared resource (like a variable or file) at a time, preventing data corruption from simultaneous access, known as a race condition

This is one of the most surprising and dangerous facts about modern programming. Both your Compiler (like `GCC` or the Java JIT) and your `CPU hardware` can—and will—reorder your instructions to make them run faster. As long as the result is the same **for a single thread**, the compiler assumes it is safe to move code around. However, in Concurrency, this can break your program in ways that are nearly impossible to debug.

## Concurrency vs Parallelism

đồng thời và parallelism song song

Node.js is fundamentally different from C, C++, Java. While C, C++, and Java are `Multi-threaded`, Node.js is `Single-threaded` but Asynchronous.

The Event Loop: Instead of giving every user a new thread (which consumes RAM), Node uses one thread and an "Event Loop." When it hits an I/O task (like reading a database), it hands the task **to the OS** and moves to the next user.

JavaScript: Because JS is single-threaded, you never have to worry about synchronized blocks or "Data Races" on variables—there is only ever one thread touching them!

It basically means you only worry about one thread in node.js (other thread is handled by the OS or environment) while in C, C++ and Java, you must manage multiple thread yourself if your want to achieve better concurrency performance.

## Stack, Heap & Shared Variables

- The Heap is a single, large memory area allocated to the entire JVM. All threads share this same space.
- Every time a new thread is created, the JVM allocates a **completely private** `Stack` for that thread. No other thread can see or touch another thread's stack.

While the object is always on the heap, the pointer (reference) to that object is often on the stack.

Local variables (The variables declared inside the body of the method) are never shared between threads and are unaffected by the memory model.

## Thread Objects

Java `threads` allow a block of code to be executed concurrently with the rest of the program.

Each thread is associated with an instance of the class `Thread`. You simply instantiate Thread each time the application needs to initiate an asynchronous task.

- An application that creates an instance of Thread must provide the code that will run in that thread. There are two ways to do this:
  1. Provide a `Runnable` object. The Runnable interface defines a single method, run, meant to contain the code executed in the thread. The `Runnable` object is passed to the `Thread` constructor, as in the `HelloRunnable` example.
  2. Subclass `Thread` (`extends`). The `Thread` class itself implements `Runnable`, though its run method does nothing. An application can subclass Thread, providing its own implementation of `run`, as in the `HelloThread` example.

```java
// cách 1
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }
}

// cách 2
public class HelloThread extends Thread {
    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new HelloThread()).start();
    }
}
```

both examples invoke `Thread.start` in order to start the new thread.

You don't call `.start()` on `main`. The JVM handles that for you automatically.

Threads are represented by the `Thread` class. The only way for a user to create a thread is to create an object of this class; each thread is associated with such an object. A thread will start when the `start()` method is invoked on the corresponding `Thread` object.

---

Outside the `main` thread, java has many system thread running in the background. So even the simplest java application is multi-thread by default.

Even a "Hello World" in Java is never truly single-threaded. While you only see your main method, the JVM starts a small "army" of background threads to manage the complex relationship between your code and the hardware.

### Sleep & Yield

`Thread.sleep` causes the current thread to suspend execution for a specified period.

The `SleepMessages` example uses sleep to print messages at four-second intervals:

```java
public class SleepMessages {
    public static void main(String args[])
        throws InterruptedException { 
        String importantInfo[] = {
            "Mares eat oats",
            "Does eat oats",
            "Little lambs eat ivy",
            "A kid will eat ivy too"
        };

        for (int i = 0;
             i < importantInfo.length;
             i++) {
            //Pause for 4 seconds
            Thread.sleep(4000);
            //Print a message
            System.out.println(importantInfo[i]);
        }
    }
}
```

Notice that main declares that it throws `InterruptedException`. This is an exception that sleep throws when another thread interrupts the current thread while `sleep` is active. Since this application has not defined another thread to cause the interrupt, it doesn't bother to catch InterruptedException.

`Thread.sleep` causes the currently executing thread to sleep (temporarily cease execution) for the specified duration. The thread does not lose ownership of any monitors.

It is important to note that neither `Thread.sleep` nor `Thread.yield` have any synchronization semantics. In particular, the compiler does not have to flush writes cached in registers out to shared memory before a call to Thread.sleep or Thread.yield, nor does the compiler have to reload values cached in registers after a call to Thread.sleep or Thread.yield. 

---

- sleep:
  * The thread state moves from RUNNABLE to TIMED_WAITINGIt is guaranteed to stop for at least the time you specify (though it might be a few milliseconds late depending on system precision).
- yield:
  * The thread says, "I'm still ready to work, but if someone else of the same priority needs the CPU, they can have my spot."
  * The thread remains `RUNNABLE`. It moves from the "Running" sub-state back to the "Ready" sub-state.
  * It doesn't necessarily stop. If no other threads are waiting, the OS might give the CPU right back to the same thread immediately.

### Interrupts

An interrupt is an indication to a thread that it should stop what it is doing and do something else. It's up to the programmer to decide exactly how a thread responds to an interrupt, but it is very common for the thread to terminate.

A thread sends an interrupt by invoking `interrupt` on the `Thread` object **for the thread to be interrupted**. For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption.

### Joins

The `join` method allows one thread to wait for the completion of another. If `t` is a Thread object whose thread is currently executing,

`t.join();`

causes the current thread to pause execution until t's thread terminates. Overloads of `join` allow the programmer to specify a waiting period. However, as with `sleep`, join is dependent on the OS for timing, so you should not assume that join will wait exactly as long as you specify.

Like `sleep`, `join` responds to an interrupt by exiting with an `InterruptedException`.

- In this example, the `main` thread wait for both threads `A` & `B` to complete before `main` can continue its execution. `main` does so in a specific, sequential order.
  * A and `B` are running at the same time (concurrently)
  * `main` stop & wait for `A` to finish; `B` is still running together with `A`
  * One `A` finishes, `main` moves to the next line, which instruct `main` to wait for `B`
    * If `B` already finish while `main` was waiting for `tA`, this line returns immediately.
    * If `B` is still working, `main` pauses again until `B` is done.

The caller of `java.lang.Thread.join()` (`main` in this case) Moves from RUNNABLE to `WAITING` while the callee threads' state (thread A & `B`) are not affected. It continues doing its work (in your case, the for loop transfers) in the `RUNNABLE` state.
 
```java
public static void main(String[] args) throws InterruptedException {
    tA.start();
    tB.start();
    // main wait for both A & B threads
    tA.join();
    tB.join();
}
```

### `Callable` interface

k

### The State model for a thread

Java has an enum called `Thread.State`.

A Java thread object is initially created in the `NEW` state. At this time, an OS thread does not yet exist (and may never exist). To create the execution thread, `Thread.start()` must be called. This signals the OS to actually create a thread.

The scheduler will place the new thread into the run queue and, at some later point, will find a core for it to run upon (some amount of waiting time may be involved if the machine is heavily loaded). From there, the thread can proceed by consuming its time allocation and be placed back into the run queue to await further processor time slices. This is the action of the forcible thread scheduling that we mentioned in section `5.1.1`.

- Throughout this scheduling process, of being placed on a core, running, and being placed back in the run queue, the Java `Thread` object remains in the `RUNNABLE` state. As well as this scheduling action, the thread itself can indicate that it isn’t able to make use of the core at this time. This can be achieved in two different ways:
  1. The program code indicates by calling `Thread.sleep()` that the thread should wait for a fixed time before continuing.
  2. The thread recognizes that it must wait until some external condition has been met and calls `Object.wait()`.
- In both cases, the thread is immediately removed from the core by the OS. However, the behavior after that point is different in each case.

In the first case, the thread is asking to sleep for a definite amount of time. The Java thread transitions into the `TIMED_WAITING` state, and the operating system sets a timer. When it expires, the sleeping thread is woken up and is ready to run again and is placed back in the run queue.

The second case is slightly different. It uses the condition aspect of Java’s per-object monitors. The thread will transition into `WAITING` and will wait indefinitely. It will not normally wake up until the operating system signals that the condition may have been met—usually by some other thread calling `Object.notify()` on the current object.

As well as these two possibilities that are under the threads control, a thread can transition into the `BLOCKED` state because it’s waiting on I/O or to acquire a lock held by another thread.

Finally, if the OS thread corresponding to a Java Thread has ceased execution, then that thread object will have transitioned into the `TERMINATED` state.

---

In Java, the RUNNABLE state is a "wrapper" that covers two different sub-states at the Operating System level: Running and Ready.

The word "Runnable" literally means "Capable of running, but currently waiting for its turn on the CPU."

- Java doesn't distinguish between a thread that is currently using the CPU and a thread that is ready to use it but is waiting for the OS Scheduler.
  * Running: The CPU is physically executing the thread's instructions (the Instruction Pointer is moving through your .text segment).
  * Ready: The thread has everything it needs to run, but the OS has "preempted" it to give another thread a turn.

## Liveness

A concurrent application's ability to execute in a timely manner is known as its `liveness`.

A live system is one in which every attempted activity eventually either progresses or fails. A system that is not live is basically stuck—it will neither progress toward success or fail.

## Deadlocks

`Deadlock` describes a situation where two or more threads are blocked forever, waiting for each other.

Another classic problem of concurrency (and not just Java’s take on it) is the deadlock.

The reason is that each thread requires the other to release the lock it holds before the transfer method can progress.

Deadlock khi xảy ra sẽ làm program never finish, giống infinite loop

Trong java có nút show `thread dump` để nhận diện lỗi deadlock. Nếu chỉ coi console thì không thấy thông tin gì nhiều.

To deal with deadlocks, one technique is to always acquire locks in the same order in every thread. In the preceding example, the first thread to start acquires them in the order `A`, `B`, whereas the second thread acquires them in the order `B`, `A`. If both threads had insisted on acquiring them in the order A, B, the deadlock would have been avoided, because the second thread would have been blocked from running at all until the first had completed and released its locks. Later in the chapter, we will show a simple way to arrange for all locks to be obtained in the same order and a way to verify that this is indeed satisfied.

## The Java Memory Model (JMM)

The Java Memory Model (JMM) is a set of rules that determines how and when different threads can see values written to shared variables by other threads.

### Happens-before Order 

Two actions can be ordered by a `happens-before relationship`. If one action happens-before another, then the first is visible to and ordered **before** the second. 

If we have two actions x and y, we write `hb(x, y)` to indicate that x happens-before y. 

- If x and y are actions of the same thread and x comes before y in `program order`, then `hb(x, y)`.

any write to a volatile field happens before every subsequent read of the same field.

## Synchronization

synchronization can introduce `thread contention`, which occurs when two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution. Starvation and livelock are forms of thread contention.

`Thread Interference` happens when two operations, running in different threads, but acting on the same data, interleave. 

`Memory consistency errors` occur when different threads have inconsistent views of what should be the same data. The causes of memory consistency errors are complex and beyond the scope of this tutorial. Fortunately, the programmer does not need a detailed understanding of these causes. All that is needed is a strategy for avoiding them.

The key to avoiding memory consistency errors is understanding the `happens-before` relationship. This relationship is simply a guarantee that memory writes by one specific statement are visible to another specific statement

---

The Java programming language provides two basic synchronization idioms: `synchronized methods` and `synchronized statements`.

Other mechanisms, such as reads and writes of `volatile` variables and the use of classes in the `java.util.concurrent` package, provide alternative ways of synchronization.

### Lock & Monitor

The Java programming language provides multiple mechanisms for communicating between threads. The most basic of these methods is `synchronization`, which is implemented using `monitors`.

Each object in Java is associated with a `monitor`, which a thread can `lock` or `unlock`. Only one thread at a time may hold a lock on a monitor. Any other threads attempting to lock that monitor are blocked until they can obtain a lock on that monitor. A thread `t` may lock a particular monitor multiple times; each unlock reverses the effect of one lock operation.

The `synchronized statement` computes a reference to an object; it then attempts to perform a lock action on that object's monitor and does not proceed further until the lock action has successfully completed. After the lock action has been performed, the body of the `synchronized` statement is executed. If execution of the body is ever completed, either normally or abruptly, an unlock action is automatically performed on that same monitor.

- A `synchronized method` automatically performs a lock action when it is invoked; its body is not executed until the lock action has successfully completed. 
  * If the method is an instance method, it locks the monitor associated with the instance for which it was invoked (that is, the object that will be known as `this` during execution of the body of the method).
  * If the method is `static`, it locks the monitor associated with the `Class` object that represents the class in which the method is defined.
- If execution of the method's body is ever completed, either normally or abruptly, an unlock action is automatically performed on that same monitor.

### Intrinsic Locks and Synchronization

Synchronization is built around an internal entity known as the `intrinsic lock` or `monitor lock` (or simply as a `monitor.`).

Every object has an intrinsic lock associated with it. By convention, a thread that needs exclusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them. A thread is said to _own_ the intrinsic lock between the time it has acquired the lock and released the lock. As long as a thread owns an intrinsic lock, no other thread can acquire the same lock. The other thread will block when it attempts to acquire the lock.

When a thread "locks" an object, it becomes the owner of that object's monitor.

An object has only one lock and one monitor. So if an object has multiple `synchronized` methods, only one thread can execute one method at a given time. Other threads are blocked, even if they try to call a completely different method.  
But other threads can call non-synchronized method while an object is locked.

A `synchronized` method only stops other threads that are trying to enter other synchronized parts of that same object. It provides zero protection against threads calling non-synchronized methods.

---

If, in the following example, one thread repeatedly calls the method `one`, and another thread repeatedly calls the method `two`, then method `two` could occasionally print a value for `j` that is greater than the value of `i`, because the example includes no synchronization and, under the rules explained in, the shared values of `i` and `j` might be updated out of order.

One way to prevent this out-or-order behavior would be to declare methods `one` and `two` to be `synchronized`. This prevents method `one` and method `two` from being executed concurrently, and furthermore guarantees that the shared values of `i` and `j` are both updated **before** method `one` returns. Therefore method `two` **never** observes a value for `j` greater than that for `i`; indeed, it always observes the same value for `i` and `j`.

Another approach would be to declare `i` and `j` to be `volatile`.

```java
class Test {
    static int i = 0, j = 0;
    static void one() { i++; j++; }
    static void two() {
        System.out.println("i=" + i + " j=" + j);
    }
}

// Using synchronized
class Test {
    static int i = 0, j = 0;
    static synchronized void one() { i++; j++; }
    static synchronized void two() {
        System.out.println("i=" + i + " j=" + j);
    }
}

// Using volatile
class Test {
    static volatile int i = 0, j = 0;
    static void one() { i++; j++; }
    static void two() {
        System.out.println("i=" + i + " j=" + j);
    }
}
```

### Fully synchronized objects

One way to achieve concurrent type safety is `fully synchronized objects`. If all of the following rules are obeyed, the class is known to be `thread-safe` and will also be `live`.

- A fully synchronized class is a class that meets all of the following conditions:
  * All fields are always initialized to a consistent state in every constructor.
  * There are no public fields.
  * Object instances are guaranteed to be consistent after returning from any nonprivate method (assuming the state was consistent when the method was called).
  * All methods provably terminate in bounded time.
  * All methods are `synchronized`.
  * No method calls another instance’s methods while in an inconsistent state.
  * No method calls any nonprivate method on the current instance while in an inconsistent state. 

This seems fantastic at first glance—the class is both safe and live. The problem comes with performance. Just because something is safe and live doesn’t mean it’s necessarily going to be very quick. You have to use `synchronized` to coordinate all the accesses (both get and put) to the balance, and that locking is ultimately going to slow you down. This is a central problem of this way of handling concurrency.

In real, larger systems, this sort of manual verification would not be possible due to the amount of code. It’s too easy for bugs to creep into larger codebases that use this approach, which is another reason that the Java community began to look for more robust approaches. 

## Wait Sets and Notification

Every **object**, in addition to having an associated monitor, has an associated `wait set`. A wait set is a set of threads.

When an object is first created, its wait set is empty. Elementary actions that add threads to and remove threads from wait sets are `atomic`. Wait sets are manipulated solely through the methods `Object.wait`, `Object.notify`, and `Object.notifyAll`.

Wait set manipulations can also be affected by the `interruption status` of a thread, and by the `Thread` class's methods dealing with interruption. Additionally, the `Thread` class's methods for sleeping and joining other threads have properties derived from those of wait and notification actions.

### Wait

Wait actions occur upon invocation of `wait()`, or the timed forms `wait(long millisecs)` and `wait(long millisecs, int nanosecs)`.

A thread returns normally from a wait if it returns without throwing an `InterruptedException`.

- `wait()`:
  * wait for a signal (notify)
  * Must be called inside a `synchronized` block.
- `sleep()`:
  * Pauses for a fixed amount of time.
  * Can be called anywhere

The invocation of `wait` does not return until another thread has issued a notification that some special event may have occurred — though NOT necessarily the event this thread is waiting for:

When a thread invokes `wait` on an object, it must own the intrinsic lock for that object — otherwise an error is thrown. Invoking `wait` inside a `synchronized` method is a simple way to acquire the intrinsic lock.

When `wait` is invoked, the thread releases the lock and suspends execution. At some future time, another thread will acquire the same lock and invoke `Object.notifyAll`, informing all threads waiting on that lock that something important has happened:

## Memory Flush

"flush" in conputer memory does not mean getting rid of the data (delete them). "flush" means to force data out of a temporary, fast storage area (like a cache or a buffer) and into its final, more permanent destination (like RAM or a Hard Drive).

- When you flush a buffer or a cache:
  * The Source is cleared: The temporary storage (cache/buffer) now has space for new data.
  * The Destination is updated: The "real" storage (RAM/Disk) now contains the most recent version of the data.

## Block-structured concurrency (pre-Java 5)

The `synchronized` keyword can be applied either to a block or to a method. It indicates that before entering the block or method, a thread must acquire the appropriate lock.

The method must acquire the lock belonging to the object instance (or the lock belonging to the `Class` object for static synchronized methods). For a `synchronized` block, the programmer should indicate which object’s lock is to be acquired.

The method must acquire the lock belonging to the object instance (or the lock belonging to the `Class` object for `static synchronized` methods). For a block, the programmer should indicate which object’s lock is to be acquired.

Only one thread can be progressing through **any** of an object’s `synchronized` blocks or methods at once; if other threads try to enter, they’re suspended **by the JVM**. This is true regardless of whether the other thread is trying to enter the same or a different synchronized block on the same object. In concurrency theory, this type of construct (the code inside a synchronized method or block) is sometimes referred to as a `critical section`, but this term is more commonly used in C++ than in Java.

Have you ever wondered why the Java keyword used for a `critical section` is `synchronized`? Why not “critical” or “locked”? What is it that’s being synchronized?

Next, we’ll return to a puzzle we posed earlier: why the Java keyword for a critical section is named synchronized. This will then lead us into a discussion of the `volatile` keyword.

We asked earlier, what is it that’s being synchronized in the code? The answer is: **the memory representation in different threads of the object being locked**. That is, after the synchronized method (or block) has completed, any and all changes that were made to the object being locked are flushed back to main memory before the lock is released.

In addition, when a synchronized block is entered, after the lock has been acquired, any changes to the locked object are read in from main memory, so the thread with the lock is synchronized to the main memory’s view of the object before the code in the locked section begins to execute.

A change to an object propagates between threads via the main memory.

---

- Only objects—not primitives—can be locked.
- Locking an array of objects doesn’t lock the individual objects.
- A `synchronized method` can be thought of as equivalent to a `synchronized (this) { ... }` block that covers the entire method (but note that they’re represented differently in bytecode).
- A `static synchronized method` locks the `Class` object, because there’s no instance object to lock.
- Synchronization in an inner class is independent of the outer class (to see why this is so, remember how inner classes are implemented).
- `synchronized` doesn’t form part of the `method signature`, so it can’t appear on a method declaration in an interface.
- Unsynchronized methods don’t look at or care about the state of any locks, and they can progress while synchronized methods are running.
- Java’s locks are `reentrant`—a thread holding a lock that encounters a synchronization point for the same lock (such as a `synchronized` method calling another `synchronized` method on the same object) will be allowed to continue. 

---

`Thread Interference` happens when two operations, running in different threads, but acting on the same data, interleave. This means that the two operations consist of multiple steps, and the sequences of steps overlap.

### The `volatile` keyword

The Java programming language allows threads to access shared variables. As a rule, to ensure that shared variables are consistently and reliably updated, a thread should ensure that it has exclusive use of such variables by obtaining a lock that, conventionally, enforces **mutual exclusion** for those shared variables.

The Java programming language provides a second mechanism, `volatile` fields, that is more convenient than locking for some purposes.

A field may be declared `volatile`, in which case the Java Memory Model ensures that all threads see a consistent value for the variable

---

- Java has had the volatile keyword since the dawn of time (Java 1.0), and it’s used as a simple way to deal with concurrent handling of object fields, including primitives. The following rules govern a volatile field:
  * The value seen by a thread is always reread from the main memory before use.
  * Any value written by a thread is always flushed through to the main memory before the bytecode instruction completes. 

This is sometimes described as being “like a tiny little synchronized block” around the single operation, but this is misleading because volatile does not involve any locking. The action of `synchronized` is to use a mutual exclusion lock on an object to ensure that only one thread can execute a synchronized method on that object. Synchronized methods can contain many read and write operations on the object, and they will be executed as an indivisible unit (from the point of view of other threads) because the results of the method executing on the object are not seen until the method exits and the object is flushed back to main memory.

The key point about `volatile` is that it allows for only one operation on the memory location, which will be immediately flushed to memory. This means either a single read, or a single write, but not more than that.

A volatile variable should be used to model a variable only where writes to the variable don’t depend on the current state (the read state) of the variable. This is a consequence of volatile guaranteeing only a single operation.  
For example, the `++` and `--` operators are not safe to use on a `volatile`, because they are equivalent to `v = v + 1` or `v = v – 1`. The increment example is a classic example of a state-dependent update.

For cases where the current state matters, you must always introduce a lock to be completely safe. So, `volatile` allows the programmer to write simplified code in some cases, but at the cost of the extra flushes on every access. Notice also that because the volatile mechanism doesn’t introduce any locks, you can’t deadlock using volatiles—only with synchronization. 

---

CPU executes the code in their cores. Fetching from RAM is slow for the CPU, so CPU cache data.

Different threads on the CPU can cache data retrieve from RAM into their own thread.

Khi một thread write value vào variable, nó vào `write buffer` trước khi được write vào main memory (RAM). Những thread khác không biết được.

To ensure that updates to variables propagate predictably to other threads, we should apply the volatile modifier to those variables.

This way, we can communicate with runtime and processor to avoid reordering any instruction involving the volatile variable. Also, processors understand that they should immediately flush any updates to these variables.

- For multithreaded applications, we need to ensure a couple of rules for consistent behaviour:
  * Mutual Exclusion – only one thread executes a critical section at a time
  * Visibility – changes made by one thread to the shared data are visible to other threads to maintain data consistency

The `synchronized` methods and blocks provide both of the above properties at the cost of application performance.

The `volatile` field is quite a useful mechanism because it can help **ensure the visibility aspect of the data change without providing mutual exclusion**. Thus, it’s useful where we’re ok with multiple threads executing a block of code in parallel, but we need to ensure the visibility property.

A Java compiler is allowed to reorder program text instructions if, in a given thread, it does not affect the execution of that thread in isolation. A shared variable that includes the volatile modifier guarantees that, for each thread, the runtime and processor carry out the instructions related to the shared variable in the same order as they appear in the program text without applying any optimizations that may reorder the instructions. **A shared variable that includes the volatile modifier guarantees that all threads see a consistent value for the shared variable. Any update to a volatile field updates the shared value of the field immediately. In other words, a different thread cannot get an inconsistent value of the shared variable after its value is updated.**

It’s important to understand that volatile guarantees a consistent value of a shared variable; however, the absence of the volatile modifier doesn’t mean or guarantee that multiple threads always get an inconsistent value of the shared variable. In the absence of the volatile modifier, other threads may occasionally get an inconsistent value of the shared variable. Therefore, let’s not expect that by removing the volatile modifier in the example application TaskRunner, the application starts to print inconsistent values for a shared variable; however, it could happen.

## Atomic classes

The package `java.util.concurrent.atomic` contains several classes that have names starting with `Atomic`, for example, `AtomicBoolean`, `AtomicInteger`, AtomicLong, and AtomicReference. These classes are one of the simplest examples of a concurrency primitive—a class that can be used to build workable, safe concurrent applications.

The point of an atomic is to provide thread-safe mutable variables. Each of the four classes provides access to a single variable of the appropriate type.