# JVM Notes

## Terminologies

- JVM: The core engine. It loads, verifies, and executes bytecode.
- JRE: JVM + Libraries (like java.util and java.io) + Other Files. It is the environment needed for someone to run a Java app.
- JDK: JRE + Development Tools (like javac). This is what you need to build a Java app.

## The language and the Platform

- The Java language—The Java language is the statically typed, object-oriented language. One obvious point about source code written in the Java language is that it’s human-readable (or it should be!).
- The Java platform—The platform is the software that provides a `runtime environment`. It’s the `JVM` that links and executes your code as provided to it in the form of (not human-readable) class files. It doesn’t directly interpret Java language source files but instead requires them to be **converted to class files first**.

The link between the language and platform is the class file `.class`.

Java source code is transformed into `.class` files, then manipulated at load time before being JIT-compiled.

`javac` compile `.java` (human-readable) into `.class`. The `.class` files are loaded into a JVM. Class loading is an essential feature of the Java platform.

Is Java a compiled (biên dịch) or interpreted language (thông dịch)? => Both.

The standard picture of Java is of a language that’s compiled into `.class` files before being run on a JVM. If pressed, many developers can also explain that bytecode starts off by being interpreted by the JVM but will undergo just-in-time (JIT) compilation at some later point.

JVM bytecode is more like a halfway house between human-readable source and machine code. In the technical terms of compiler theory, bytecode is really a form of `intermediate language (IL)` rather than actual machine code. This means that the process of turning Java source into bytecode isn’t really compilation in the sense that a C++ or a Go programmer would understand it, and `javac` isn’t a compiler in the same sense as `gcc` is—it’s really a class file generator for Java source code. The real compiler in the Java ecosystem is the `JIT compiler`.

The existence of the source code compiler, `javac`, leads many developers to think of Java as a static, compiled language. One of the big secrets is that at runtime, the Java environment is actually very dynamic.

## Class files and bytecode

Java `.class` files contains bytecode.

class loading is the process by which the JVM locates and activates a new type for use in a running program. Central to that discussion are the `Class` objects that represent types in the JVM. These concepts build into the major language feature known as reflection (or Core Reflection).

class loading is the process by which new classes are incorporated into a running JVM process.

The arrival of the modular JVM introduces some (small) changes to class loading (prior to Java8)

`javap` is used for examining and dissecting class files

`Java Hotspot`

### Class loading and class objects

Java is not the only language that uses the JVM. As long as a language can be compiled into Java Bytecode (the `.class` file format), the JVM will run it. Example: Kotlin, Scala, Groovy

A `.class` file defines a type for the JVM, complete with fields, methods, inheritance information, annotations, and other metadata. A class is the fundamental unit of program code that the Java platform will understand, accept, and execute.

One way of looking at the JVM is that it is an execution container. In this view, the purpose of the JVM is to consume class files and execute the bytecode they contain. To achieve this, the JVM must retrieve the contents of the class file as a data stream of bytes, convert it to a useable form, and add it to the running state.

## The Java Virtual Machine

The Java Virtual Machine knows nothing of the Java programming language, only of a particular binary format, the class file format. A class file contains `Java Virtual Machine instructions` (or `bytecodes`) and a symbol table, as well as other ancillary information.

Compiled code to be executed by the Java Virtual Machine is represented using a hardware- and operating system-independent binary format, typically (but not necessarily) stored in a file, known as the class file format. The class file format precisely defines the representation of a class or interface