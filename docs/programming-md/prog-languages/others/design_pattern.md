# Design Pattern

## Singleton Pattern

Singleton is a creational design pattern that lets you ensure that a class has only one instance and provide a global access point to this instance

Singleton là 1 trong 5 design pattern của nhóm `Creational Design Pattern`.

## Factory Method

k

## Closure

A `closure` is an inner function that is created and returned from the outer function. Moreover, it requires the inner function to use the variable(s) in the outer function’s scope, a technique called nonlocal variable binding.

```python
# A higher-order function that returns a function as output
def outer(a):
    b = 5
    def inner(b):
        return a + b
    return inner
```

In the body of the outer function, we create an inner function; the inner function uses parameters that belong to the outer function; and the outer function returns the inner function as its output.

When we create a function by calling the outer function, we’re creating a closure. If you inspect the closure, you see that it is indeed the inner function created in the outer function, and you can call the closure too:

```python
>>> closure = outer(100)
>>> closure
<function outer.<locals>.inner at 0x7f89a812d5a0>
>>> closure()
105
```

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