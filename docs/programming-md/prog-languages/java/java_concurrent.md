# Java Concurrent Programming

## Basics & Terminologies

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

### Sleep

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

### Interrupts

An interrupt is an indication to a thread that it should stop what it is doing and do something else. It's up to the programmer to decide exactly how a thread responds to an interrupt, but it is very common for the thread to terminate.

A thread sends an interrupt by invoking `interrupt` on the `Thread` object **for the thread to be interrupted**. For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption.

### Joins

The `join` method allows one thread to wait for the completion of another. If `t` is a Thread object whose thread is currently executing,

`t.join();`

causes the current thread to pause execution until t's thread terminates. Overloads of `join` allow the programmer to specify a waiting period. However, as with `sleep`, join is dependent on the OS for timing, so you should not assume that join will wait exactly as long as you specify.

Like sleep, join responds to an interrupt by exiting with an InterruptedException.

### `Callable` interface

k

## Liveness

A concurrent application's ability to execute in a timely manner is known as its `liveness`.

A live system is one in which every attempted activity eventually either progresses or fails. A system that is not live is basically stuck—it will neither progress toward success or fail.

`Deadlock` describes a situation where two or more threads are blocked forever, waiting for each other.

## The Java Memory Model (JMM)

The Java Memory Model (JMM) is a set of rules that determines how and when different threads can see values written to shared variables by other threads.

## Synchronization

 synchronization can introduce `thread contention`, which occurs when two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution. Starvation and livelock are forms of thread contention.

`Thread Interference` happens when two operations, running in different threads, but acting on the same data, interleave. 

`Memory consistency errors` occur when different threads have inconsistent views of what should be the same data. The causes of memory consistency errors are complex and beyond the scope of this tutorial. Fortunately, the programmer does not need a detailed understanding of these causes. All that is needed is a strategy for avoiding them.

The key to avoiding memory consistency errors is understanding the `happens-before` relationship. This relationship is simply a guarantee that memory writes by one specific statement are visible to another specific statement

---

The Java programming language provides two basic synchronization idioms: `synchronized methods` and `synchronized statements`.

### Intrinsic Locks and Synchronization

Synchronization is built around an internal entity known as the intrinsic lock or `monitor lock`.

Every object has an intrinsic lock associated with it. By convention, a thread that needs exclusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them. A thread is said to _own_ the intrinsic lock between the time it has acquired the lock and released the lock. As long as a thread owns an intrinsic lock, no other thread can acquire the same lock. The other thread will block when it attempts to acquire the lock.

## Block-structured concurrency (pre-Java 5)

The `synchronized` keyword can be applied either to a block or to a method. It indicates that before entering the block or method, a thread must acquire the appropriate lock.

The method must acquire the lock belonging to the object instance (or the lock belonging to the `Class` object for `static synchronized` methods). For a block, the programmer should indicate which object’s lock is to be acquired.

Only one thread can be progressing through any of an object’s synchronized blocks or methods at once; if other threads try to enter, they’re suspended by the JVM. This is true regardless of whether the other thread is trying to enter the same or a different synchronized block on the same object.

- Only objects—not primitives—can be locked.
- Locking an array of objects doesn’t lock the individual objects.
- A static synchronized method locks the Class object, because there’s no instance object to lock.

---

`Thread Interference` happens when two operations, running in different threads, but acting on the same data, interleave. This means that the two operations consist of multiple steps, and the sequences of steps overlap.

## Atomic classes

The package `java.util.concurrent.atomic` contains several classes that have names starting with `Atomic`, for example, `AtomicBoolean`, `AtomicInteger`, AtomicLong, and AtomicReference. These classes are one of the simplest examples of a concurrency primitive—a class that can be used to build workable, safe concurrent applications.

The point of an atomic is to provide thread-safe mutable variables. Each of the four classes provides access to a single variable of the appropriate type.