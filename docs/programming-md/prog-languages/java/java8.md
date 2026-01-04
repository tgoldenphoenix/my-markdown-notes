# Modern Java: Java 8 & Beyond

## Java Version & History

- Companies and tutorials often stick to Long-Term Support (LTS) versions, because they’re supported for many years.
  - Java 8 → LTS
  - Java 11 → LTS
  - Java 17 → LTS
- Non-LTS versions (9, 10, 12, 13, 14, 15, 16) only had 6 months of support, so many skipped them.

- Java 8 (or Java SE 8, codenamed "Oak") was officially released by Oracle on March 18, 2014.
  - stream API
  - Lambda expression & functional interfaces: provide support for functional programming paradigms
  - method references (the double colon operator (`::`))
  - Optional class: A new class for handling null values safely, reducing NullPointerException errors and improving code robustness
  - static and default methods in interfaces

Java 8 was released in March 2014, Java 9 in September 2017, Java 10 in March 2018, and Java 11 planned for September 2018.

Java 8 provides two concise techniques to pass code to methods: method references, lambdas.

## Functional Programming in Java

Two core ideas from functional programming that are now part of Java: using methods and lambdas as first-class values, and the idea that calls to functions or methods can be efficiently and safely executed in parallel in the absence of mutable shared state. Both of these ideas are exploited by the new Streams API we described earlier.

To pass behavior to stream methods, you must provide behavior that is safe to execute concurrently on different pieces of the input. Typically this means writing code that doesn’t access shared mutable data to do its job. Sometimes these are referred to as `pure functions` or side-effect-free functions or stateless functions

The previous parallelism arises only by assuming that multiple copies of your piece of code can work independently. If there’s a shared variable or object, which is written to, then things no longer work. What if two processes want to modify the shared variable at the same time?

The two points (no shared mutable data and the ability to pass methods and functions—code—to other methods) are the cornerstones of what’s generally described as the paradigm of functional programming.  
In contrast, in the `imperative programming` (lập trình mệnh lệnh) paradigm you typically describe a program in terms of a sequence of statements that mutate state.

Since Java 8, Method & lambda are first-class citizen in Java. Class are second-class citizen.

The idea is that calls to functions or methods can be efficiently and safely executed in parallel in the absence of mutable shared state.

### Behavior parameterization

`Behavior parameterization` is a software development pattern that lets you handle frequent requirement changes. In a nutshell, it means taking a block of code and making it available without executing it. This block of code can be called later by other parts of your programs, which means that you can defer the execution of that block of code. For instance, you could pass the block of code as an argument to another method that will execute it later. As a result, the method’s behavior is parameterized based on that block of code.

This pattern is historically verbose in Java. Lambda expressions in Java 8 onward tackle the problem of verbosity.

It is used to tell a `Thread` to execute a block of code or even perform GUI event handling

Nếu dùng normal value like `String`, `Int`, `boolean` (`value parameterization`) để parameterize method thì không có gì đáng để bàn rồi.

---

You need a better way than adding lots of parameters to cope with changing requirements. One possible solution is to model your selection criteria: you’re working with apples and returning a `boolean` based on some attributes of `Apple`. For example, is it green? Is it heavier than 150 g? We call this a `predicate` (a function that returns a `boolean`). Let’s therefore define an interface to **model the selection criteria**:

```java
public interface ApplePredicate{
    boolean test (Apple apple);
}
```

You can now declare multiple implementations of `ApplePredicate` to represent different selection criteria:

```java
public class AppleHeavyWeightPredicate implements ApplePredicate {
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}

public class AppleGreenColorPredicate implements ApplePredicate {
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}
```

What you just did is related to the `strategy design pattern` (see <http://en.wikipedia.org/wiki/Strategy_pattern>), which lets you define a family of algorithms, encapsulate each algorithm (called a strategy), and select an algorithm at run time. In this case the family of algorithms is `ApplePredicate` and the different strategies are `AppleHeavyWeightPredicate` and `AppleGreenColorPredicate`.

But how can you make use of the different implementations of `ApplePredicate`? You need your `filterApples` method to accept an `ApplePredicate` objects to test a condition on an `Apple`. This is what behavior parameterization means: the ability to tell a method to take multiple behaviors (or strategies) as parameters and use them internally to accomplish different behaviors.

Nếu parameter là một interface thì arguments có thể là class implement cái interface đó.

---

Unfortunately, because the `filterApples` method can only take objects, you have to wrap that code inside an `ApplePredicate` object. What you’re doing is similar to passing code inline, because you’re passing a `boolean` expression through an object that implements the `test` method. You’ll see that by using lambdas, you can directly pass the expression `RED.equals(apple.getColor()) && apple.getWeight() > 150` to the `filterApples` method without having to define multiple `ApplePredicate` classes. This removes unnecessary verbosity.

---

behavior parameterization is great because it enables you to separate the logic of iterating the collection to filter and the behavior to apply on each element of that collection. As a consequence, you can reuse the same method and give it different behaviors to achieve different things

### abstracting over `List` type

There’s one more step that you can do in your journey toward abstraction. At the moment, the filterApples method works only for Apple. But you can also abstract on the List type to go beyond the problem domain you’re thinking of, as shown:

```java
public interface Predicate<T> {
    boolean test(T t);
}
public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> result = new ArrayList<>();
    for(T e: list) {
        if(p.test(e)) {
            result.add(e);
        }
    }
    return result;
}
```

You can now use the method filter with a List of bananas, oranges, Integers, or Strings! Here’s an example, using lambda expressions:

```java
List<Apple> redApples =
  filter(inventory, (Apple apple) -> RED.equals(apple.getColor()));
List<Integer> evenNumbers =
  filter(numbers, (Integer i) -> i % 2 == 0);
```

### Real-world examples

Many methods in the Java API can be parameterized with different behaviors. These methods are often used together with anonymous classes. We show four examples, which should solidify the idea of passing code for you: sorting with a `Comparator`, executing a block of code with `Runnable`, returning a result from a task using `Callable`, and GUI event handling.

---

Sorting with a `Comparator`

From Java 8, a `List` comes with a `sort` method (you could also use `Collections.sort`). The behavior of `sort` can be parameterized using a `java.util.Comparator` object, which has the following interface:

```java
// java.util.Comparator
public interface Comparator<T> {
    int compare(T o1, T o2);
}

// an anonymous class
inventory.sort(new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2) {
      return a1.getWeight().compareTo(a2.getWeight());
    }
});

// lambda expression
inventory.sort(
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

Even though `Comparator` is an interface, you can still `new` it. This is a hidden mechanic in Java. This only works for anonymous class passed as argument like the code example above.

- When you write `new Comparator<Apple>() { ... }`, the Java compiler sees that you are trying to instantiate an interface. Since that’s impossible, it immediately does these things for you:
  - It creates a hidden, unnamed class file (usually named something like `YourClassName$1.class`).
  - It makes that hidden class `implement Comparator<Apple>`.
  - It puts your `compare` method inside that hidden class.
  - It then calls `new` on that hidden class, not the interface.

---

Executing a block of code with Runnable

Java threads allow a block of code to be executed concurrently with the rest of the program. But how can you tell a thread what block of code it should run? Several threads may each run different code. What you need is a way to represent a piece of code to be executed later. Until Java 8, only objects could be passed to the `Thread` constructor, so the typical clumsy usage pattern was to pass an anonymous class containing a `run` method that returns `void` (no result). Such anonymous classes implement the Runnable interface.

In Java, you can use the Runnable interface to represent a block of code to be executed; note that the code returns void (no result):

```java
// java.lang.Runnable
public interface Runnable {
    void run();
}
```

You can use this interface to create threads with your choice of behavior, as follows:

```java
Thread t = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello world");
    }
});
```

But since Java 8 you can use a lambda expression, so the call to Thread would look like this:

```java
Thread t = new Thread(() -> System.out.println("Hello world"));
```

## Lambdas Expressions

- Java có: `lambdas` and `Anonymous Classes`
- Javascript có khái niệm `anonymous function`
- In Java, lambda có thể coi như là `anonymous function` methods without declared names, but which can also be passed as arguments to a method as you can with an anonymous class.
- While JavaScript is a `functions-first` language where a function can exist on its own, Java is strictly object-oriented. This means a Java "anonymous function" must always be tied to a Functional Interface (an interface with exactly one method).

Behavior parameterization could, prior to Java 8, be encoded using anonymous classes. `Behavior parameterization` ngắn gọn khi là có thể pass a method to another method mà không cần phải tạo object (hay anonymous class).

The Streams API is built on the idea of passing code to parameterize the behavior of its operations.

Lambdas technically don’t let you do anything that you couldn’t do prior to Java 8. But you no longer have to write clumsy code using anonymous classes to benefit from behavior parameterization! Lambda expressions will encourage you to adopt the style of behavior parameterization

You could define a method `add1` inside a class `MyMathsUtils` and then write `MyMaths-Utils::add1!` Yes, you could, but the new lambda syntax is more concise for cases where you don’t have a convenient method and class available.

Từ interface `ApplePredicate` có thể có two classes implement: `AppleGreenColorPredicate` & `AppleHeavyWeightPredicate`.

- Mức độ verbose từ cao xuống thấp:
  - Classes
  - Anonymous class
  - lambdas

A lambda expression can be understood as a concise representation of an anonymous function that can be passed around. It doesn’t have a name, but it has a list of parameters, a body, a return type, and also possibly a list of exceptions that can be thrown.

A lambda expression can be passed as argument to a method or stored in a variable.

```java
// before
Comparator<Apple> byWeight = new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2){
        return a1.getWeight().compareTo(a2.getWeight());
    }
};
// after (with lambda expression)
Comparator<Apple> byWeight =
    (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

Notice that here, you can `new Comparator` which is a functional interface, not a class. It works for this case. I think it must be a functional interface for this technique to work.

five examples of valid lambda expressions in Java 8.

```java
// Takes one parameter of type String and returns an int. It has no return statement as return is implied.
(String s) -> s.length()
  
// Takes one parameter of type Apple and returns a boolean (whether the apple is heavier than 150 g).
(Apple a) -> a.getWeight() > 150
  
// Takes two parameters of type int and returns no value (void return). Its body contains two statements (not an expression).
(int x, int y) -> {
    System.out.println("Result:");
    System.out.println(x + y);
}

// Takes no parameter and returns the int 42
() -> 42

// Takes two parameters of type Apple and returns an int representing the comparison of their weights
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())
```

```java
// expression-style lambda
(parameters) -> expression
// block-style lambda
(parameters) -> { statements; }

// no parameters and returns void. 
() -> {}
// no parameters and returns a String as an expression.
() -> "Raoul"
// no parameters and returns a String (using an explicit return statement, within a block).  
() -> { return "Mario"; }

// curly braces are required as follows: (Integer i) -> { return "Alan" + i; }.
(Integer i) -> return "Alan" + i; // INVALID

// remove the curly braces and semicolon as follows: (String s) -> "Iron Man"
(String s) -> { "Iron Man"; } // INVALID

// Creating objects
() -> new Apple(10)

// Consuming from an object
(Apple a) -> {
  System.out.println(a.getWeight());
}
```

### Where and how to use lambdas

You can use a lambda expression in the context of a `functional interface`. You can pass a lambda as argument to a method that expects an object of the type of a functional interface.

```java
List<Apple> greenApples =
        filter(inventory, (Apple a) -> GREEN.equals(a.getColor()));
```

- A `Funtional Interface` is an interface that specifies **exactly one abstract method**. And Lambda is a shortcut to define an implementation of a FI.
- FIs can contains other types of method (static method, default method).

A functional interface có thể `extends` từ một `tagging interface`:

```java
// java.awt.event.ActionListener functional interface
public interface ActionListener extends EventListener {
    void actionPerformed(ActionEvent e);
}
```

---

```java
public interface Adder {
    int add(int a, int b);
}
public interface SmartAdder extends Adder {
    int add(double a, double b);
```

Only `Adder` is a functional interface.  
`SmartAdder` isn’t a functional interface because it specifies two abstract methods called `add` (one is inherited from `Adder`).

---

 Lambda expressions let you provide the implementation of the abstract method of a functional interface directly inline and **treat the whole expression as an instance of a functional interface** (more technically speaking, an instance of a **concrete implementation** of the functional interface). You can achieve the same thing with an anonymous inner class, although it’s clumsier: you provide an implementation and instantiate it directly inline.

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
process(r1); // Hellow World 1
process(r2); // Hellow World 2
process(() -> System.out.println("Hello World 3")); // Prints “Hello World 3” with a lambda passed directly
```

Lambda don't need access modifier, no return type, no method name

---

If a lambda exceeds a few lines in length (so that its behavior isn’t instantly clear), you should instead use a method reference to a named method with a descriptive name instead of using an anonymous lambda. Code clarity should be your guide.

You’ve seen that you can abstract over behavior and make your code adapt to requirement changes, but the process is verbose because you need to declare multiple classes that you instantiate only once. Let’s see how to improve that.

### Function Descriptor

The signature of the abstract method of the functional interface describes the signature of the lambda expression. We call this abstract method a `function descriptor`. For example, the `Runnable` interface can be viewed as the signature of a function that accepts nothing and returns nothing (`void`) because it has only one abstract method called `run`, which accepts nothing and returns nothing (`void`).

a lambda expression can be assigned to a variable or passed to a method expecting a functional interface as argument, provided the lambda expression has the same signature as the abstract method of the functional interface.

```java
public void process(Runnable r) {
    r.run();
}
process(() -> System.out.println("This is awesome!!")); // This is awesome!!
```

The lambda expression `() -> System.out.println("This is awesome!!")` takes no parameters and returns `void`. This is exactly the signature of the `run` method defined in the `Runnable` interface.

```java
public Callable<String> fetch() {
  return () -> "Tricky example ;-)";
}
```

This example is valid because the return type of the method `fetch` is `Callable<String>`. `Callable<String>` defines a method with the signature `() -> String` when `T` is replaced with `String`. Because the lambda `() -> "Tricky example ;-)"` has the signature `() -> String`, the lambda can be used in this context.

### Anonymous Classes

Java has mechanisms called `anonymous classes`, which let you declare and instantiate a class at the same time. They enable you to improve your code one step further by making it a little more concise. But they’re not entirely satisfactory.

Anonymous classes are like the `local classes` (a named class defined in a block - could be class or interface) that you’re already familiar with in Java. But anonymous classes don’t have a name. They allow you to declare and instantiate a class at the same time. In short, they allow you to create ad hoc implementations.

But anonymous classes are still not good enough. First, they tend to be bulky because they take a lot of space. Second, many programmers find them confusing to use.

Verbosity in general is bad; it discourages the use of a language feature because it takes a long time to write and maintain verbose code, and it’s not pleasant to read! Good code should be easy to comprehend at a glance.

Using anonymous class, you still have to create an object and explicitly implement a method to define a new behavior (for example, the method `test` for `Predicate` or the method `handle` for `EventHandler`).

To achieve behaviour parameterization, from verbose to concise: name class > anonymous class > lambda

### Type checking, type inference, and restrictions

There’s no need to fully understand the next section right away, and you may wish to come back to it later and move on to section 3.6 about method references.

the Java compiler could infer the types of the parameters of a lambda expression by using the context in which the lambda appears.

```java
// Comparator represents a function descriptor (T, T) -> int
inventory.sort((Apple a1, Apple a2)
                -> a1.getWeight().compareTo(a2.getWeight())
);
// equivalent
inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

### Method References

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
  - You can pass in a method reference
  - Or you can pass in a lambda expression
  - Cả 2 techniques này có mục đích chung là: provide the implementation for the one abstract method required by the functional interface

---

You use lambda expressions to create anonymous methods. Sometimes, however, a lambda expression does nothing but call an existing method. In those cases, it's often clearer to refer to the existing method by name. Method references enable you to do this; they are compact, easy-to-read lambda expressions for methods that already have a name.

```java
Arrays.sort(rosterAsArray,
    (a, b) -> Person.compareByAge(a, b)
);

Arrays.sort(rosterAsArray, Person::compareByAge);
```

The method reference `Person::compareByAge` is semantically the same as the lambda expression `(a, b) -> Person.compareByAge(a, b)`

- Method references let you reuse existing method definitions and pass them like lambdas. In some cases they appear more readable and feel more natural than using lambda expressions.  
- Method references can be seen as shorthand for lambdas calling only a specific method.

The basic idea is that if a lambda represents “call this method directly,” it’s best to refer to the method by name rather than by a description of how to call it. Indeed, a method reference lets you create a lambda expression from an existing method implementation.

When you need a method reference, the target reference is placed before the delimiter `::` and the name of the method is provided after it.

For example, `Apple::getWeight` is a method reference to the method `getWeight` defined in the Apple class. (Remember that no brackets are needed after getWeight because you’re not calling it at the moment, you’re merely quoting its name.)  
This method reference is shorthand for the lambda expression `(Apple apple) -> apple.getWeight()`.

- `() -> Thread.currentThread().dumpStack()` == `Thread.currentThread()::dumpStack`
- `(str, i) -> str.substring(i)` = `String::substring`
- `(String s) -> System.out.println(s)` = `System.out::println`
- `(String s) -> this.isValidName(s)` = `this::isValidName`

---

- There are three main kinds of method references:
  1. A method reference to a static method (for example, the method `parseInt` of `Integer`, written `Integer::parseInt`)
  2. A method reference to an instance method of an arbitrary type (for example, the method length of a String, written `String::length`)
  3. A method reference to an **instance method of an existing object or expression** (for example, suppose you have a local variable `expensiveTransaction` that holds an object of type `Transaction`, which supports an instance method `getValue`; you can write `expensiveTransaction::getValue`)

- `(String s) -> s.toUpperCase()` = `String::toUpperCase`
- `() -> expensiveTransaction.getValue()` = `expensiveTransaction::getValue`

```java
private boolean isValidName(String string) {
    return Character.isUpperCase(string.charAt(0));
}
// Predicate<String>
filter(words, this::isValidName)
```

### Constructor references

You can create a reference to an existing constructor using its name and the keyword `new` as follows: `ClassName::new`. It works similarly to a reference to a static method.

For example, suppose there’s a zero-argument constructor. This fits the signature `() -> Apple` of `Supplier`; you can do the following:

```java
// Constructor reference to the default Apple() constructor
Supplier<Apple> c1 = Apple::new;
// Calling Supplier’s get method produces a new Apple.
Apple a1 = c1.get();

// equivalent code
Supplier<Apple> c1 = () -> new Apple(); // Lambda expression to create an Apple using the default constructor
Apple a1 = c1.get();
```

If you have a constructor with signature `Apple(Integer weight)`, it fits the signature of the `Function` interface, so you can do this:

```java
Function<Integer, Apple> c2 = Apple::new;
Apple a2 = c2.apply(110);

// equivalent
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);
Apple a2 = c2.apply(110);
```

### Compose lambda expressions

```java
// Reversed order
// sort the apples by decreasing weight
inventory.sort(comparing(Apple::getWeight).reversed());
// .reversed() a a default method of Comparator
```

```java
// Chaining Comparators
inventory.sort(comparing(Apple::getWeight)
         .reversed() // Sorts by decreasing weight
         .thenComparing(Apple::getCountry)); // Sorts further by country when two apples have same weight
```

---

The `Predicate` interface includes three methods that let you reuse an existing Predicate to create more complicated ones: `negate`, `and`, and `or`.

```java
// an apple that is not red
Predicate<Apple> notRedApple = redApple.negate(); // Produces the negation of the existing Predicate object redApple
// an apple is both red and heavy
Predicate<Apple> redAndHeavyApple =
    redApple.and(apple -> apple.getWeight() > 150);
// apples that are red and heavy (above 150 g) or only green apples
Predicate<Apple> redAndHeavyAppleOrGreen =
    redApple.and(apple -> apple.getWeight() > 150)
            .or(apple -> GREEN.equals(a.getColor()));
```

Note that the precedence of methods `and` and `or` in the chain is from left to right—there is no equivalent of bracketing. So `a.or(b).and(c)` must be read as `(a || b) && c`. Similarly, `a.and(b).or(c)` must be read as as `(a && b) || c`.

---

The Function interface comes with two default methods for this, andThen and `compose`, which both return an instance of `Function`.

The method `andThen` returns a function that first applies a given function to an input and then applies another function to the result of that application. For example, given a function `f` that increments a number `(x -> x + 1)` and another function `g` that multiples a number by 2, you can combine them to create a function `h` that first increments a number and then multiplies the result by 2:

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g);
int result = h.apply(1); // This returns 4.
```

## Functional Interface

- Some Functional Interfaces in the Java API:
  - `java.util.Comparator<T>`
  - `java.lang.Runnable`
  - `java.util.concurrent.Callable<V>`
  - `java.util.function.Predicate<T>`: A `predicate` in Java is a function that returns a `boolean` value. Ví dụ method truyền `ApplePredicate` vào `filterApple()` as a filtering criteria.
  - `Supplier<T>` with function descriptors `() -> T`
  - `IntBinaryOperator` used to combine two values `(int a, int b) -> a * b`

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

---

In order to use different lambda expressions, you need a set of functional interfaces that can describe common `function descriptors` (lambda-expression signatures). Several functional interfaces are already available in the Java API.  
The Java library designers for Java 8 have helped you by introducing several new functional interfaces inside the `java.util.function` package.

### Predicate

The word `predicate` is often used in mathematics to mean something function-like that takes a value for an argument and returns true or false.

The `java.util.function.Predicate<T>` interface defines an abstract method named `test` that accepts an object of generic type `T` and returns a `boolean`. It’s available out of the box!

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
public <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> results = new ArrayList<>();
    for(T t: list) {
        if(p.test(t)) {
            results.add(t);
        }
    }
    return results;
}
Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

### Consumer

The `java.util.function.Consumer<T>` interface defines an abstract method named `accept` that takes an object of generic type `T` and returns no result (`void`). You might use this interface when you need to access an object of type `T` and perform some operations on it.

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
public <T> void forEach(List<T> list, Consumer<T> c) {
    for(T t: list) {
        c.accept(t);
    }
}
// print all the elements of the list
forEach(
         Arrays.asList(1,2,3,4,5),
        (Integer i) -> System.out.println(i)
       );
```

### Function

The `java.util.function.Function<T, R>` interface defines an abstract method named `apply` that takes an object of generic type `T` as input and returns an object of generic type `R`. You might use this interface when you need to define a lambda that maps information from an input object to an output (for example, extracting the weight of an apple or mapping a string to its length). In the listing that follows, we show how you can use it to create a method map to transform a list of `Strings` into a list of `Integers` containing the length of each String.

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
public <T, R> List<R> map(List<T> list, Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for(T t: list) {
        result.add(f.apply(t));
    }
    return result;
}
// [7, 2, 6]
List<Integer> l = map(
                       Arrays.asList("lambdas", "in", "action"),
                       (String s) -> s.length()
               );
```

### Primitive specializations

To refresh a little: every Java type is either a reference type (for example, `Byte`, `Integer`, `Object`, `List`) or a primitive type (for example, `int`, `double`, `byte`, `char`). But generic parameters (for example, the `T` in `Consumer<T>`) can be bound only to reference types. This is due to how generics are internally implemented. As a result, in Java there’s a mechanism to convert a primitive type into a corresponding reference type. This mechanism is called `boxing`. The opposite approach (converting a reference type into a corresponding primitive type) is called `unboxing`. Java also has an autoboxing mechanism to facilitate the task for programmers: boxing and unboxing operations are done automatically. For example, this is why the following code is valid (an `int` gets boxed to an `Integer`):

```java
List<Integer> list = new ArrayList<>();
for (int i = 300; i < 400; i++){
    list.add(i);
}
```

But this comes with a performance cost. Boxed values are a wrapper around primitive types and are stored on the heap. Therefore, boxed values use more memory and require additional memory lookups to fetch the wrapped primitive value.

Java 8 also added a specialized version of the functional interfaces we described earlier in order to avoid autoboxing operations when the inputs or outputs are primitives. For example, in the following code, using an `IntPredicate` avoids a boxing operation of the value `1000`, whereas using a `Predicate<Integer>` would box the argument `1000` to an `Integer` object:

```java
public interface IntPredicate {
    boolean test(int t);
}
IntPredicate evenNumbers = (int i) -> i % 2 == 0;
evenNumbers.test(1000); // True (no boxing)

Predicate<Integer> oddNumbers = (Integer i) -> i % 2 != 0;
oddNumbers.test(1000); // False (boxing)
```

In general, the appropriate primitive type precedes the names of functional interfaces that have a specialization for the input type parameter (for example, `DoublePredicate`, `IntConsumer`, `LongBinaryOperator`, `IntFunction`, and so on). The `Function` interface also has variants for the output type parameter: `ToIntFunction<T>`, `IntToDoubleFunction`, and so on.

### What about exceptions?

You have two options if you need the body of a lambda expression to throw an exception: define your own functional interface that declares the checked exception, or wrap the lambda body with a try/catch block.

Decalre a new functional interface Buffered-Reader-Processor that explicitly declared an IOException:

```java
@FunctionalInterface
public interface BufferedReaderProcessor {
    String process(BufferedReader b) throws IOException;
}
BufferedReaderProcessor p = (BufferedReader br) -> br.readLine();
```

But you may be using an API that expects a functional interface such as `Function<T, R>` and there’s no option to create your own. You’ll see in the next chapter that the Streams API makes heavy use of the functional interfaces from table 3.2. In this case, you can explicitly catch the checked exception:

```java
Function<BufferedReader, String> f =
  (BufferedReader b) -> {
    try {
      return b.readLine();
    }
    catch(IOException e) {
      throw new RuntimeException(e);
    }
  };
```

## `Optional<T>`

Common functional languages (SML, OCaml, Haskell) also provide further constructs to help programmers. One of these is avoiding `null` by explicit use of more descriptive data types.

Java 8 introduced the `Optional<T>` class that, if used consistently, can help you avoid `null-pointer exceptions`. It’s a container object that may or may not contain a value.  
The `Optional<T>` class includes methods to explicitly deal with the case where a value is absent, and as a result you can avoid null-pointer exceptions. It uses the type system to allow you to indicate when a variable is anticipated to potentially have a missing value.

## The Streams API

A `stream` is a sequence of data items that are conceptually produced one at a time. A program might read items from an input stream one by one and similarly write items to an output stream.

**Stream operations** are divided into intermediate operations (return `Stream<T>`) and terminal operations (return a result of definite type). Intermediate operations allow chaining.  
Operations on streams don’t change the source.

The Streams API provides a different way to process data in comparison to the Collections API.  
Using a collection, you’re managing the iteration process **yourself**. You need to iterate through the elements one by one using a for-each loop processing them in turn. We call this way of iterating over data `external iteration`.  
In contrast, using the Streams API, you don’t need to think in terms of loops. The data processing happens internally inside the library. We call this idea `internal iteration`.

Collections is mostly about storing and accessing data, whereas Streams is mostly about **describing computations on data**. The key point here is that the Streams API allows and encourages the elements within a stream to be processed in parallel.

Although it may seem odd at first, often the **fastest** way to filter a collection (for example, to use filterApples in the previous section on a list) is to convert it to a stream, process it in parallel, and then convert it back to a list.

By default, Java Streams are sequential.

`java.util` & `java.util.stream` are two different package

Java stream let you manipulate collections of data in a `declarative` way (you express a query rather than code an ad hoc implementation for it).  
In addition, streams can be processed in parallel transparently, without you having to write any multithreaded code! This increase performance when you process a large collection.

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

The operations `filter`, `map`, and `forEach` are aggregate operations. `Aggregate operations` process elements from a stream, NOT directly from a collection (which is the reason why the first method invoked in this example is `.stream()`).  
A stream is a sequence of elements. Unlike a collection, it is not a data structure that stores elements. Instead, a stream carries values from a source, such as collection, through a `pipeline`. A pipeline is a sequence of stream operations, which in this example is filter-map-forEach. In addition, aggregate operations typically **accept lambda expressions** as parameters, enabling you to customize how they behave.

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
  - (1) class đó phải khai báo cụ thể muốn dùng default method của interface nào hoặc
  - (2) class đó phải tự provide implementation của riêng nó cho default method (giống abstract method).

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

## Enhanced type inference (the `var` keyword)

Java has historically had a reputation as a verbose language. However, in recent versions, the language has evolved to make more and more use of `type inference`. This feature of the `source code compiler` enables the compiler to work out some of the type information in programs automatically. As a result, it doesn’t need to be told everything explicitly.

The aim of type inference is to reduce boilerplate content, remove duplication, and allow for more concise and readable code.

This trend started with Java 5, when generic methods were introduced. Generic methods permit a very limited form of type inference of generic type arguments, so that instead of having to explicitly provide the exact type that is needed, like this:

```java
List<Integer> empty = Collections.<Integer>emptyList();
// the generic type parameter can be omitted on the right-hand side, like so:
List<Integer> empty = Collections.emptyList();
```

Code below declares that you have some users, whom you identify by userid (which is an integer), and each user has a set of properties (modeled as a map of string to strings) specific to that user. The compiler work out the type information on the right side.

```java
// prior to java 7
Map<Integer, Map<String, String>> usersLists =
                        new HashMap<Integer, Map<String, String>>();
// from java 7 onward
Map<Integer, Map<String, String>> usersLists = new HashMap<>();
```

Because the shortened type declaration looks like a diamond, this form is called “diamond syntax.”

In Java 8, more type inference was added to support the introduction of lambda expressions, like this example where the type inference algorithm can conclude that the type of `s` is a `String`:

```java
Function<String, Integer> lengthFn = s -> s.length();
```

---

In modern Java, type inference has been taken one step further, with the arrival of `Local Variable Type Inference (LVTI)`, otherwise known as `var`. This feature was **added in Java 10** and allows the developer to infer the types of variables, instead of the types of values, like this:

```java
var names = new ArrayList<String>();
```

This is implemented by making `var` a reserved, “magic” type name rather than a language keyword. Developers can still in theory use `var` as the name of a variable, method, or package.

The intention of `var` is to reduce verbosity in Java code and to be familiar to programmers coming to Java from other languages. It does NOT introduce dynamic typing, and all Java variables **continue to have static types at all times**—you just don’t need to write them down explicitly in all cases.

Type inference in Java is local, and in the case of var, the type inference algorithm examines only the declaration of the local variable. This means it cannot be used for fields, method arguments, or return types.

`var` is implemented solely in the source code compiler (`javac`) and has no runtime or performance effect whatsoever.

## Small changes in Java 11

Collections factories (JEP 213)

add simple factory methods to the relevant interfaces, exploiting the fact that Java 8 added the ability to have static methods on interfaces.

```java
Set<String> set = Set.of("a", "b", "c");
var list = List.of("x", "y");
```

For maps, a different factory method, `ofEntries()`, is used in combination with a static helper method, entry()

```java
Map.ofEntries(
    entry(k1, v1),
    entry(k2, v2),
    // ...
    entry(kn, vn));
```

the factory methods produce instances of immutable types. These class are new implementations of the Java Collections interfaces that are immutable—they are not the familiar, mutable classes (such as `ArrayList` and `HashMap`). Attempts to modify instances of these types will result in an exception being thrown.

```java
jshell> var ints = List.of(2, 3, 5, 7);
ints ==> [2, 3, 5, 7]
 
jshell> ints.getClass();
$2 ==> class java.util.ImmutableCollections$ListN
```

---

Single-file source-code programs (JEP 330)

The usual way that Java programs are executed is by compiling source code to a class file and then starting up a virtual machine process that acts as an execution container to interpret the bytecode of the class.  
This is very different from languages like Python, Ruby, and Perl, where the source code of a program is interpreted directly. The Unix environment has a long history of these types of scripting languages, but Java has not traditionally been counted among them.

With the arrival of JEP 330, Java 11 offers a new way to execute programs. Source code can be compiled in memory and then executed by the interpreter without ever producing a `.class` file on disk. This gives a user experience that is like Python and other scripting languages.

## Java modules

Java 9 introduces Java Platform Modules (also known as JPMS, Jigsaw, or just “modules”). This is a major enhancement and change to the Java platform that had been discussed for many years.

This will enable you to use JDK and third-party modules in your build as well as packaging apps or libraries as modules.

I don't need to know this for now...

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
