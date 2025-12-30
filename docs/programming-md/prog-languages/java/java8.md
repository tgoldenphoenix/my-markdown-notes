# Modern Java & Java 8

## Java Version & History

- Companies and tutorials often stick to Long-Term Support (LTS) versions, because they’re supported for many years.
  * Java 8 → LTS
  * Java 11 → LTS
  * Java 17 → LTS
- Non-LTS versions (9, 10, 12, 13, 14, 15, 16) only had 6 months of support, so many skipped them.

- Java 8 (or Java SE 8, codenamed "Oak") was officially released by Oracle on March 18, 2014.
  * stream API
  * Lambda expression & functional interfaces: provide support for functional programming paradigms
  * method references (the double colon operator (`::`))
  * Optional class: A new class for handling null values safely, reducing NullPointerException errors and improving code robustness
  * static and default methods in interfaces

Java 8 was released in March 2014, Java 9 in September 2017, Java 10 in March 2018, and Java 11 planned for September 2018.

Java 8 provides two concise techniques to pass code to methods: method references, lambdas.

## Functional Programming in Java

Two core ideas from functional programming that are now part of Java: using methods and lambdas as first-class values, and the idea that calls to functions or methods can be efficiently and safely executed in parallel in the absence of mutable shared state. Both of these ideas are exploited by the new Streams API we described earlier.

To pass behavior to stream methods, you must provide behavior that is safe to execute concurrently on different pieces of the input. Typically this means writing code that doesn’t access shared mutable data to do its job. Sometimes these are referred to as `pure functions` or side-effect-free functions or stateless functions

The previous parallelism arises only by assuming that multiple copies of your piece of code can work independently. If there’s a shared variable or object, which is written to, then things no longer work. What if two processes want to modify the shared variable at the same time?

The two points (no shared mutable data and the ability to pass methods and functions—code—to other methods) are the cornerstones of what’s generally described as the paradigm of functional programming.  
In contrast, in the `imperative programming` (lập trình mệnh lệnh) paradigm you typically describe a program in terms of a sequence of statements that mutate state.

Since Java 8, Method & lambda are first-class citizen in Java. Class are second-class citizen.

## Method References

Method Reference (giá trị tham chiếu tới hàm): The double colon operator (`::`))

When we are using a method reference – the target reference is placed before the delimiter :: and the name of the method is provided after it.  
`Computer::getAge;` => a method reference to the method getAge defined in the `Computer` class

---

Code to filter all the hidden files in a directory.

```java
// Prior to java 8
// required to pass an object reference to File.listFiles()
File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
    public boolean accept(File file) {
        return file.isHidden();
    }
});

// After java 8
// passing a method reference instead of an object reference to File.listFiles()
// no need to use a new FileFilter object reference
File[] hiddenFiles = new File(".").listFiles(File::isHidden);
```

Analogous to using an `object reference` (giá trị tham chiếu đối tượng) when you pass an object around (and object references are created by `new`), in Java 8 when you write `File::isHidden`, you create a `method reference` (giá trị tham chiếu tới một hàm), which can similarly be passed around. 

Vì mình đọc API doc và biết trong class `java.io.File` có sẵn method `isHidden()` nên dùng method reference là cách làm nhanh nhất.  
Nếu giả sử không có sẵn method `isHidden()` thì mình sẽ nghĩ tới dùng lambda (anonymous function). The lambda syntax is more concise for cases where you don’t have a convenient method and class available.

- Method reference: treating existing named methods inside class as first-class citizen, passing them into other function like normal values (named function as value).
- lambda expression: using anonymous methods (no name) and passing them into other funciton like first-class citizen (anonymous function as value)

Programs using these concepts are said to be written in `functional-programming style`; this phrase means “writing programs that pass functions around as first-class values.”

- Khi một method (like `filterApples()`) accept a functional interface as its parameter (as the filtering condition):
  * You can pass in a method reference
  * Or you can pass in a lambda expression
  * Cả 2 techniques này có mục đích chung là: provide the implementation for the one abstract method required by the functional interface

---

You use lambda expressions to create anonymous methods. Sometimes, however, a lambda expression does nothing but call an existing method. In those cases, it's often clearer to refer to the existing method by name. Method references enable you to do this; they are compact, easy-to-read lambda expressions for methods that already have a name.

```java
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);

Arrays.sort(rosterAsArray, Person::compareByAge);
```

The method reference `Person::compareByAge` is semantically the same as the lambda expression `(a, b) -> Person.compareByAge(a, b)`

Method references let you reuse existing method definitions and pass them like lambdas. In some cases they appear more readable and feel more natural than using lambda expressions.

Method references can be seen as shorthand for lambdas calling only a specific method.

When you need a method reference, the target reference is placed before the delimiter `::` and the name of the method is provided after it.

For example, `Apple::getWeight` is a method reference to the method `getWeight` defined in the Apple class. (Remember that no brackets are needed after getWeight because you’re not calling it at the moment, you’re merely quoting its name.)  
This method reference is shorthand for the lambda expression `(Apple apple) -> apple.getWeight()`. 

- `(str, i) -> str.substring(i)` = `String::substring`
- `(String s) -> System.out.println(s)` = `System.out::println`
- `(String s) -> this.isValidName(s)` = `this::isValidName`

---

- There are three main kinds of method references:
  1. A method reference to a static method (for example, the method `parseInt` of `Integer`, written `Integer::parseInt`)
  2. A method reference to an instance method of an arbitrary type (for example, the method length of a String, written String::length)
  3. A method reference to an **instance method of an existing object or expression** (for example, suppose you have a local variable `expensiveTransaction` that holds an object of type `Transaction`, which supports an instance method `getValue`; you can write `expensiveTransaction::getValue`)

## Lambdas Expressions

- Java có: `lambdas` and `Anonymous Classes`
- Javascript có khái niệm `anonymous function`
- While JavaScript is a `functions-first` language where a function can exist on its own, Java is strictly object-oriented. This means a Java "anonymous function" must always be tied to a Functional Interface (an interface with exactly one method).

Behavior parameterization could, prior to Java 8, be encoded using anonymous classes. `Behavior parameterization` ngắn gọn khi là có thể pass a method to another method mà không cần phải tạo object (hay anonymous class).

The Streams API is built on the idea of passing code to parameterize the behavior of its operations.

Lambdas technically don’t let you do anything that you couldn’t do prior to Java 8. But you no longer have to write clumsy code using anonymous classes to benefit from behavior parameterization! Lambda expressions will encourage you to adopt the style of behavior parameterization

You could define a method `add1` inside a class `MyMathsUtils` and then write `MyMaths-Utils::add1!` Yes, you could, but the new lambda syntax is more concise for cases where you don’t have a convenient method and class available.

Từ interface `ApplePredicate` có thể có two classes implement: `AppleGreenColorPredicate` & `AppleHeavyWeightPredicate`.

- Mức độ verbose từ cao xuống thấp:
  * Classes
  * Anonymous class
  * lambdas

---

You can use a lambda expression in the context of a functional interface. In the code shown here, you can pass a lambda as second argument to the method filter because it expects an object of type Predicate<T>, which is a functional interface. 

```java
List<Apple> greenApples =
        filter(inventory, (Apple a) -> GREEN.equals(a.getColor()));
```

- A `Funtional Interface` is an interface that specifies exactly one abstract method. And Lambda is a shortcut to define an implementation of a FI.
- FIs can contains other types of method (static method, default method).

 Lambda expressions let you provide the implementation of the abstract method of a functional interface directly inline and treat the whole expression as an instance of a functional interface (more technically speaking, an instance of a concrete implementation of the functional interface). You can achieve the same thing with an anonymous inner class, although it’s clumsier: you provide an implementation and instantiate it directly inline.

The following code is valid because `Runnable` is a functional interface defining only one abstract method, `run`:

```java
// uses a lambda
Runnable r1 = () -> System.out.println("Hello World 1");
// uses an anonymous class
Runnable r2 = new Runnable() {
    public void run() {
        System.out.println("Hello World 2");
    }
};
public static void process(Runnable r) {
    r.run();
}
process(r1);
process(r2);
process(() -> System.out.println("Hello World 3"));
```

Lambda don't need access modifier, no return type, no method name

---

If a lambda exceeds a few lines in length (so that its behavior isn’t instantly clear), you should instead use a method reference to a named method with a descriptive name instead of using an anonymous lambda. Code clarity should be your guide.

## Functional Interface

- Functional Interfaces in the Java API:
  * `java.util.Comparator`
  * `java.lang.Runnable`
  * `java.util.concurrent.Callable`
  * `java.util.function.Predicate<T>`: A `predicate` in Java is a function that returns a `boolean` value. Ví dụ method truyền `ApplePredicate` vào `filterApple()` as a filtering criteria.

The signature of the abstract method of a functional interface is called a `function descriptor`.

Java 8 introduced several **pre-defined standard functional interfaces** in the `java.util.function` package to support lambda expressions and method references.

Because a functional interface contains only one abstract method, you can omit the name of that method when you implement it. To do this, instead of using an anonymous class expression, you use a lambda expression

Remember, to use a lambda expression, you need to implement a functional interface. Hoặc dùng những cái có sẵn hoặc tự tạo custom functional interface.

```java
// The method as written in the class
public static void processPersons(
    List<Person> roster,
    Predicate<Person> tester,
    Consumer<Person> block) {
        for (Person p : roster) {
            if (tester.test(p)) {
                block.accept(p);
            }
        }
}

// This is how you call the method in action
processPersons(
     roster,
     p -> p.getGender() == Person.Sex.MALE
         && p.getAge() >= 18
         && p.getAge() <= 25,
     p -> p.printPerson()
);
```

```java
// method in class
public static void processPersonsWithFunction(
    List<Person> roster,
    Predicate<Person> tester,
    Function<Person, String> mapper,
    Consumer<String> block) {
    for (Person p : roster) {
        if (tester.test(p)) {
            String data = mapper.apply(p);
            block.accept(data);
        }
    }
}

// call the method
processPersonsWithFunction(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

```java
// generic method in class
public static <X, Y> void processElements(
    Iterable<X> source,
    Predicate<X> tester,
    Function <X, Y> mapper,
    Consumer<Y> block) {
    for (X p : source) {
        if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
        }
    }
}

// call the generic method above
processElements(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);
```

The following example uses aggregate operations to print the e-mail addresses of those members contained in the collection roster who are eligible for Selective Service:

```java
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));
```

### Predicate

The word `predicate` is often used in mathematics to mean something function-like that takes a value for an argument and returns true or false.

## `Optional<T>`

Java 8 introduced the `Optional<T>` class that, if used consistently, can help you avoid null-pointer exceptions. It’s a container object that may or may not contain a value. `Optional<T>` includes methods to explicitly deal with the case where a value is absent, and as a result you can avoid null-pointer exceptions. It uses the type system to allow you to indicate when a variable is anticipated to potentially have a missing value.

## The Streams API

 A `stream` is a sequence of data items that are conceptually produced one at a time. A program might read items from an input stream one by one and similarly write items to an output stream.
 
**Stream operations** are divided into intermediate operations (return `Stream<T>`) and terminal operations (return a result of definite type). Intermediate operations allow chaining.  
Operations on streams don’t change the source.

The Streams API provides a different way to process data in comparison to the Collections API.  
Using a collection, you’re managing the iteration process **yourself**. You need to iterate through the elements one by one using a for-each loop processing them in turn. We call this way of iterating over data `external iteration`.  
In contrast, using the Streams API, you don’t need to think in terms of loops. The data processing happens internally inside the library. We call this idea `internal iteration`.

Collections is mostly about storing and accessing data, whereas Streams is mostly about **describing computations on data**. The key point here is that the Streams API allows and encourages the elements within a stream to be processed in parallel.

Although it may seem odd at first, often the **fastest** way to filter a collection (for example, to use filterApples in the previous section on a list) is to convert it to a stream, process it in parallel, and then convert it back to a list.

---

```java
long count = list.stream().distinct().count();
```

So, the `distinct()` method represents an intermediate operation, which creates a new stream of unique elements of the previous stream. And the `count()` method is a `terminal operation`, which returns stream’s size.

Stream API helps to substitute for, for-each, and while loops. It allows concentrating on operation’s logic, but not on the iteration over the sequence of elements.

```java
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));
```

The operations filter, map, and `forEach` are aggregate operations. `Aggregate operations` process elements from a stream, NOT directly from a collection (which is the reason why the first method invoked in this example is `.stream()`).  
A stream is a sequence of elements. Unlike a collection, it is not a data structure that stores elements. Instead, a stream carries values from a source, such as collection, through a `pipeline`. A pipeline is a sequence of stream operations, which in this example is filter-map-forEach. In addition, aggregate operations typically **accept lambda expressions** as parameters, enabling you to customize how they behave.

---

By default, Java Streams are sequential.

`java.util` & `java.util.stream` are two different package

---

```java
// java.util.stream package, Stream<T> interface
static <T> Collection<T> filter(Collection<T> c, Predicate<T> p);

filterApples(inventory, (Apple a) -> a.getWeight() > 150 );
```

- Stream API ra đời để giải quyết 2 vấn đề một cách tối ưu hơn:
  1. Better data-processing pattern (filter, extract, grouping)
  2. parallelized those processing to utilize multicore computers in an easier way than using `synchronized`

### Parallelism Processing, Exploiting Multicore computers

As a second pain point of working with collections, think for a second about how you would process the list of transactions if you had a vast number of them; how can you process this huge list? A single CPU wouldn’t be able to process this large amount of data, but you probably have a multicore computer on your desk. Ideally, you’d like to share the work among the different CPU cores available on your machine to reduce the processing time. In theory, if you have eight cores, they should be able to process your data eight times as fast as using one core, because they work in parallel.

This naming is unfortunate in some ways. Each of the cores in a multicore chip is a full-fledged CPU. But the phrase multicore CPU has become common, so core is used to refer to the individual CPUs.

All new desktop and laptop computers are multicore computers. Instead of a single CPU, they have four or eight or more CPUs (usually called Cores5). The problem is that a classic Java program uses just a single one of these cores, and the power of the others is wasted. Similarly, many companies use computing clusters (computers connected together with fast networks) to be able to process vast amounts of data efficiently. Java 8 facilitates new programming styles to better exploit such computers.

---

The problem is that exploiting parallelism by writing multithreaded code (using the Threads API from previous versions of Java) is difficult. You have to think differently: threads can access and update shared variables at the same time. As a result, data could change unexpectedly if not coordinated properly. This model is harder to think about than a step-by-step sequential model.

Thread có memory của riêng nó và nó cache data value trong memory của riêng nó. Khi cần thì nó write value vào RAM. Multiple thread write value to the same object stored inside RAM can cause problems **IF** they’re NOT synchronized properly.

Traditionally via the keyword `synchronized`, but many subtle bugs arise from its misplacement. Java 8’s Stream-based parallelism encourages a functional programming style where `synchronized` is rarely used; it focuses on partitioning the data rather than coordinating access to it.

filtering a list on two CPUs could be done by asking one CPU to process the first half of a list and the second CPU to process the other half of the list. This is called the `forking step` (1). The CPUs then filter their respective half-lists (2). Finally (3), one CPU would `join` the two results.

Again, we’ll just say “parallelism almost for free” and provide a taste of how you can filter heavy apples from a list sequentially or in parallel using streams and a lambda expression.

```java
// Here’s an example of sequential processing:
import static java.util.stream.Collectors.toList;
List<Apple> heavyApples =
    inventory.stream().filter((Apple a) -> a.getWeight() > 150)
                      .collect(toList());

// And here it is using parallel processing:
List<Apple> heavyApples =
    inventory.parallelStream().filter((Apple a) -> a.getWeight() > 150)
                              .collect(toList());
```

Không cần suy nghĩ về thread, `synchronized` làm gì cho mệt

You should actually use `stream()` as your default and only switch to `parallelStream()` when you have a specific, proven reason to do so.  
In many cases, `parallelStream()` is actually slower than a standard `stream()` due to the overhead of managing multiple threads.

---

Parallelism in Java and no shared mutable state

 First, the library handles partitioning—breaking down a big stream into several smaller streams to be processed in parallel for you.  
 Second, this parallelism almost for free from streams, works only if the methods passed to library methods like filter don’t interact (for example, by having mutable shared objects). But it turns out that this restriction feels natural to a coder (see, by way of example, our `Apple::isGreenApple` example). Although the primary meaning of functional in functional programming means “using functions as first-class values,” it often has a secondary nuance of “no interaction during execution between components.”
 
## Default Methods and Java modules

Prior to Java 8 you can **update (or evolve) an interface** only if you update all the classes that implement it—a logistical nightmare! This issue is resolved in Java 8 by `default methods`. Java 8 added default methods to support **evolvable interfaces**.

- Like regular interface methods, `default methods` are implicitly `public`; there’s no need to specify the public modifier.
- Unlike regular interface methods, we declare them with the `default` keyword at the beginning of the method signature, and they **provide an implementation**. Classes that implement the interface không cần phải implement default methods nữa.
- Mục đích của default method là để cho phép add more method to interface mà không phải viết thêm code trong các class that had already implemented that interface. It allowes interfaces to evolve without breaking existing implementations, improving the flexibility of the language

- When a class implements several interfaces that define the same default methods thì: 
  * (1) class đó phải khai báo cụ thể muốn dùng default method của interface nào hoặc
  * (2) class đó phải tự provide implementation của riêng nó cho default method (giống abstract method).

an interface can now contain method signatures for which an implementing class doesn’t provide an implementation. Then who implements them? The missing method bodies are given as part of the interface (hence default implementations) rather than in the implementing class.  
This provides a way for an interface designer to enlarge an interface beyond those methods that were originally planned—without breaking existing code. Java 8 allows the existing default keyword to be used in interface specifications to achieve this.

For example, in Java 8, you can call the sort method directly on a list. This is made possible with the following default method in the Java 8 List interface, which calls the static method Collections.sort:

```java
default void sort(Comparator<? super E> c) {
    Collections.sort(this, c);
}
```

This means any concrete classes of List don’t have to explicitly implement `sort`, whereas in previous Java versions such concrete classes would fail to recompile unless they provided an implementation for sort.

---

Default methods enables interfaces and their libraries to evolve with less fuss and less recompilation; it also explains the modules addition to Java 9, which enables components of large Java systems to be specified more clearly than “just a JAR file of packages.”

modules containing collections of packages

---

static methods in Interfaces

- **Static method** trong interface giống static method trong class: dùng `static` keyword, interface phải provide implementation không được hứa, static method belong to the interface.
- The same can pretty much be done with abstract classes. The main difference is that abstract classes can have constructors, state, and behavior.

Java 9 provides a module system that provide you with syntax to define modules containing collections of packages—and keep much better control over visibility and namespaces. Modules enrich a simple JAR-like component with structure, both as user documentation and for machine checking

relatively few programmers will need to write default methods themselves and because they facilitate program evolution rather than helping write any particular program

---

But wait a second. A single class can implement multiple interfaces, right? If you have multiple default implementations in several interfaces, does that mean you have a form of multiple inheritance in Java? Yes, to some extent. We show in chapter 13 that there are some rules that prevent issues such as the infamous `diamond inheritance problem` in C++.
