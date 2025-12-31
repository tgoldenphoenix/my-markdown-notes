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

Most implementations of the Java virtual machine run as **a single process**. Multiprocess applications are beyond the scope of this lesson.

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
  1. Provide a `Runnable` object. The Runnable interface defines a single method, run, meant to contain the code executed in the thread. The Runnable object is passed to the Thread constructor, as in the HelloRunnable example:

```java
public class HelloRunnable implements Runnable {
    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }

}
```

### `Callable` interface

k

## Liveness

A concurrent application's ability to execute in a timely manner is known as its `liveness`.

## The Java Memory Model (JMM)

The Java Memory Model (JMM) is a set of rules that determines how and when different threads can see values written to shared variables by other threads.

## Block-structured concurrency (pre-Java 5)

The `synchronized` keyword can be applied either to a block or to a method. It indicates that before entering the block or method, a thread must acquire the appropriate lock.

The method must acquire the lock belonging to the object instance (or the lock belonging to the `Class` object for `static synchronized` methods). For a block, the programmer should indicate which object’s lock is to be acquired.

Only one thread can be progressing through any of an object’s synchronized blocks or methods at once; if other threads try to enter, they’re suspended by the JVM. This is true regardless of whether the other thread is trying to enter the same or a different synchronized block on the same object.

- Only objects—not primitives—can be locked.
- Locking an array of objects doesn’t lock the individual objects.
- A static synchronized method locks the Class object, because there’s no instance object to lock. 

---

`Thread Interference` happens when two operations, running in different threads, but acting on the same data, interleave. This means that the two operations consist of multiple steps, and the sequences of steps overlap.