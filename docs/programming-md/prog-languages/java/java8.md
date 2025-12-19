# Modern Java & Java 8

## Java version & History

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

## method References

Method Reference: The double colon operator (`::`))

When we are using a method reference – the target reference is placed before the delimiter :: and the name of the method is provided after it.  
`Computer::getAge;` => a method reference to the method getAge defined in the `Computer` class

## Functional Programming in Java

Two core ideas from functional programming that are now part of Java: using methods and lambdas as first-class values, and the idea that calls to functions or methods can be efficiently and safely executed in parallel in the absence of mutable shared state. Both of these ideas are exploited by the new Streams API we described earlier.

## `Optional<T>`

Java 8 introduced the `Optional<T>` class that, if used consistently, can help you avoid null-pointer exceptions. It’s a container object that may or may not contain a value. `Optional<T>` includes methods to explicitly deal with the case where a value is absent, and as a result you can avoid null-pointer exceptions. It uses the type system to allow you to indicate when a variable is anticipated to potentially have a missing value.

## The Streams API

**Stream operations** are divided into intermediate operations (return `Stream<T>`) and terminal operations (return a result of definite type). Intermediate operations allow chaining.  
Operations on streams don’t change the source.

The Streams API provides a different way to process data in comparison to the Collections API.  
Using a collection, you’re managing the iteration process yourself. You need to iterate through the elements one by one using a for-each loop processing them in turn. We call this way of iterating over data **external iteration**.  
In contrast, using the Streams API, you don’t need to think in terms of loops. The data processing happens internally inside the library. We call this idea internal iteration.

Collections is mostly about storing and accessing data, whereas Streams is mostly about describing computations on data.

Although it may seem odd at first, often the fastest way to filter a collection (for example, to use filterApples in the previous section on a list) is to convert it to a stream, process it in parallel, and then convert it back to a list.

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

The operations filter, map, and forEach are aggregate operations. Aggregate operations process elements from a stream, not directly from a collection (which is the reason why the first method invoked in this example is stream). A stream is a sequence of elements. Unlike a collection, it is not a data structure that stores elements. Instead, a stream carries values from a source, such as collection, through a pipeline. A pipeline is a sequence of stream operations, which in this example is filter- map-forEach. In addition, aggregate operations typically accept lambda expressions as parameters, enabling you to customize how they behave.

## Lambdas Expressions

- Java có: `lambdas` and `Anonymous Classes`.
- Javascript có khái niệm `anonymous function`
- While JavaScript is a "functions-first" language where a function can exist on its own, Java is strictly object-oriented. This means a Java "anonymous function" must always be tied to a Functional Interface (an interface with exactly one method).

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

### Functional Interface

- Functional Interfaces in the Java API:
  * `java.util.Comparator`
  * `java.lang.Runnable`
  * `java.util.concurrent.Callable`
  * `Predicate`: A `predicate` in Java is a function that returns a `boolean` value. Ví dụ method truyền `ApplePredicate` vào `filterApple()` as a filtering criteria.

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

### Method Reference

You use lambda expressions to create anonymous methods. Sometimes, however, a lambda expression does nothing but call an existing method. In those cases, it's often clearer to refer to the existing method by name. Method references enable you to do this; they are compact, easy-to-read lambda expressions for methods that already have a name.

```java
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);

Arrays.sort(rosterAsArray, Person::compareByAge);
```

The method reference Person::compareByAge is semantically the same as the lambda expression (a, b) -> Person.compareByAge(a, b)

There are four kinds of method references:

## Default Methods and Java modules

Prior to Java 8 you can update an interface only if you update all the classes that implement it—a logistical nightmare! This issue is resolved in Java 8 by default methods. Java 8 added default methods to support **evolvable interfaces**.

- Like regular interface methods, `default methods` are implicitly `public`; there’s no need to specify the public modifier.
- Unlike regular interface methods, we declare them with the `default` keyword at the beginning of the method signature, and they **provide an implementation**. Classes that implement the interface không cần phải implement default methods nữa.
- Mục đích của default method là để cho phép add more method to interface mà không phải viết thêm code trong các class that had already implemented that interface. It allowes interfaces to evolve without breaking existing implementations, improving the flexibility of the language

- When a class implements several interfaces that define the same default methods thì: 
  * (1) class đó phải khai báo cụ thể muốn dùng default method của interface nào hoặc
  * (2) class đó phải tự provide implementation của riêng nó cho default method (giống abstract method).

---

modules containing collections of packages

---

static methods in Interfaces

- **Static method** trong interface giống static method trong class: dùng `static` keyword, interface phải provide implementation không được hứa, static method belong to the interface.
- The same can pretty much be done with abstract classes. The main difference is that abstract classes can have constructors, state, and behavior.

## Thread

Java `threads` allow a block of code to be executed concurrently with the rest of the program.

The Runnable interface represents a block of code to be executed; note that the code returns void (no result).

```java
// java.lang.Runnable
public interface Runnable {
    void run();
}

Thread t = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello world");
    }
});
```

### `Callable` interface

k