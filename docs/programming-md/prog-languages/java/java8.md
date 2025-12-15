# Java 8

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

### Lambda expressions & functional interfaces

Lambda in Java can only be used with a **Funtional Interface** which is an interface with only ONE abstract method. Lambda is a shortcut to define an implementation of a FI.
FIs can contains other types of method (static method, default method).

Lambda don't need access modifier, no return type, no method name

### method references (the double colon operator (`::`))

When we are using a method reference – the target reference is placed before the delimiter :: and the name of the method is provided after it.  
`Computer::getAge;` => a method reference to the method getAge defined in the `Computer` class

## stream API

**Stream operations** are divided into intermediate operations (return Stream<T>) and terminal operations (return a result of definite type). Intermediate operations allow chaining.  
Operations on streams don’t change the source.

```java
long count = list.stream().distinct().count();
```

So, the `distinct()` method represents an intermediate operation, which creates a new stream of unique elements of the previous stream. And the count() method is a terminal operation, which returns stream’s size.

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

## Funtional Interfaces & Lambda Expressions

A lambda is an anonymous function that we can handle as a first-class language citizen. For instance, we can pass it to or return it from a method.

Before Java 8, we would usually create a class for every case where we needed to encapsulate a single piece of functionality. This implied a lot of unnecessary boilerplate code to define something that served as a primitive function representation.

A **functional interface** is an interface that contains exactly one abstract method. This single abstract method makes the interface suitable for use with lambda expressions and method references. Functional interfaces can have multiple default or static methods, but only one abstract method.

Java 8 introduced several **pre-defined standard functional interfaces** in the `java.util.function` package to support lambda expressions and method references.

Lambda expressions are used to provide implementations for the abstract method inside functional interfaces.

Lambda expressions enable you to treat functionality as method argument, or code as data.  
Lambda expressions let you express instances of single-method classes more compactly.

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

### method reference

You use lambda expressions to create anonymous methods. Sometimes, however, a lambda expression does nothing but call an existing method. In those cases, it's often clearer to refer to the existing method by name. Method references enable you to do this; they are compact, easy-to-read lambda expressions for methods that already have a name.

```java
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);

Arrays.sort(rosterAsArray, Person::compareByAge);
```

The method reference Person::compareByAge is semantically the same as the lambda expression (a, b) -> Person.compareByAge(a, b)

There are four kinds of method references:

