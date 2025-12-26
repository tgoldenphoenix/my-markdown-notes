# Python Notes

## Installation

[uv](https://docs.astral.sh/uv/) is a Python package and project manager, written in Rust.

A Python interpreter is a program that reads and executes Python code. It acts as both a translator and a runtime environment, converting the high-level Python code you write into machine-readable instructions that your computer's processor can understand and execute.

`CPython`, written in C, is the most common and reference implementation of the Python interpreter.

- Python is included by default on almost every Linux and MacOS system, but you might want to use a different version than the default.
- Python isn’t usually included by default on WindowsPython isn’t usually included by default on Windows

`IPython` (Interactive Python) is an enhanced interactive shell built on top of the standard Python interpreter.

### `python` vs `python3` command line utilities

Tại sao lại có 2 commands: `python --version`, `python3 --version`

- The `python` command typically referred to the Python 2.x interpreter.
- The `python3` command specifically referred to the Python 3.x interpreter. This distinction was crucial when both Python 2 and Python 3 were commonly installed on a system, especially on Linux distributions where system utilities might rely on Python 2.

- In modern environments:
  - Python 2 has reached its end-of-life, and Python 3 is the current and actively developed version.
  - On many newer systems and installations, the `python` command is often aliased or symlinked to python3, meaning both commands will invoke the Python 3 interpreter.
  - However, this is not universally true, and it's still possible to encounter systems where python might still point to an older Python 2 installation.

---

Virtual Environment

A best practice among Python developers is to use a project-specific virtual environment.

## `uv` Commands

`pip` install packages

`venv` (or virtual env) for creating virtual environment

The old way of managing a python project.

```bash
$ mkdir old_way
$ cd old-way/

# Create a virtual environment called .venv
$ python3 -m venv .venv

# Activate the virtual environment
$ source .venv/bin/activate

# install packages flask & requests
$ pip install flask requests

$ touch main.py

$ pip freeze > requirements.txt
```

Using `uv`

```bash
$ uv init new_app
$ cd new_app

# Install packages
$ uv add flask requests

# not `python3 main.py`
$ uv run main.py
```

`uv` automatically create a virtual environment when we install package

`uv tree` show the dependency tree

In the old method, nếu xóa `venv` sẽ mất package. Because `uv` store package information in `pyproject.toml & .lock` file, deleting venv will not cause any damage.

`uv sync` create the `venv` using the `.lock` file.

## Python Command-Line Programs

In Python, `REPL` is an acronym for Read, Evaluate, Print, and Loop. Developers use REPL Python to communicate with the Python Interpreter.  
In contrast to running a Python file, you can input commands in the REPL and see the results displayed immediately. The Python REPL also lets you print out object and method help, a list of all the accessible methods, and much more.

Python REPL = the interactive Python interpreter

This chapter focus on command-line programs written in python. Command-line techniques are very useful when you need to process large numbers of files.

`python script.py`

All of the arguments on the command line are in a list that can be accessed via `sys.argv`. Note that to access this list, you first need to import the sys module.

`python script2.py arg1 arg2 3`

---

Executing code only as main script

```python
if __name__ == '__main__':
    main()
else:
    # module-specific initialization code if any
```

If a file with this structure is called as a script, the variable `__name__` is set to `__main__`, which means that the controlling function, main, will be called. If the script has been imported into a module by some other script, its name will be its filename, and the code won’t be executed. On the other hand, if we include the totally optional and less often used else block, that will only be executed if the file has been imported as a module.

This technique prevent code from being executed when a file is imported as a module.

## Python Basics

Run `python3` in the terminal to run python in the terminal. Use `<C-d>` or type `exit()` to leave the Python prompt and return to a terminal prompt.

Use `"""` or three backticks to create multi-line comments in Python.

The `"""` is also used to create multi-line string literals.

## Built-in Data Types

In Python, an int is an object. Unlike languages like Java or C++, Python does not have the concept of primitive types. Every value you interact with—whether it is an integer, a boolean, or even a function—is a full-blown object.

- `bool`
- `str`: a string

---

Numeric Types

There are three distinct numeric types: integers, floating-point numbers, and complex numbers (`int`, `float`, `complex`).

---

Collections

There are 4 built-in data types in Python used to store collections of data: Tuple, Dictionary, List, Set

Using literals for instantiation

```python
list_obj = [1, 2, 3] # list literal

tuple_obj = (404, "Connection Error") # tuple literal

dict_obj = {"one": 1, "two": 2}

set_obj = {1, 2, 3}
```

## Dictionary & Set

`Dictionaries` are used to store data values in `key:value` pairs. A dictionary is a collection which is ordered*, changeable and do not allow duplicates.

What makes `dict` different from `list`, `tuple`, and set is the fact that it contains key-value pairs instead of individual objects.

- Dictionaries are changeable, meaning that we can change, add or remove items after the dictionary has been created.
- As of Python version 3.7, dictionaries are ordered. In Python 3.6 and earlier, dictionaries are unordered.
- Duplicate keys will overwrite existing values (because of the hashing table mechanism). A key of `1.0` float will overwrite the key of `1` integer.

Written with curly brackets `{}`

`dict.keys(), dict.values(), dict.items()` do not return `list` objects. They’re `dict_ keys`, `dict_values`, and dict_items, respectively. What’s most special about these data types is the fact that they’re all `dynamic dictionary view objects`. When the `dict` is updated, the view objects are updated too.  
This dynamic provides great convenience when we access a dictionary’s data because the data is in perfect sync with the dict object.

Always use view objects to access a dict’s data because these view objects are dynamic; they will update when the dictionary’s data is updated.

- `dictionary.get(keyname, value)`
  * `keyname`: Required. The keyname of the item you want to return the value from
  * `value`: Optional. A value to return if the specified key does not exist. 

---

`urgencies["Laundry"] == 3` => This syntax to access dict values is called `Subscript notation`.

When you’re accessing a key that doesn’t exist in the dictionary, you encounter the `KeyError` exception

The `dict.get()` method takes input the key and a default value when the key doesn’t exist. When the default argument is omitted, Python uses `None` as the default value. The following code snippet shows some examples:

The get method has the advantage of not raising `KeyError` when the key isn’t in the dictionary. More importantly, it allows you to set a proper default value as the fallback value. You can use get whenever you retrieve values from dictionaries, but I prefer subscript notation, which I find to be more readable.

---

what makes `dict.setdefault()` differ from `get` is that when you call `setdefault`, an extra operation (`dict[key] = default_value`) occurs when the key isn’t in the dictionary:

Avoid using the setdefault method, as it can set the missing key’s value in an unexpected way. Use a more explicit approach, such as the `get` method

---

`dictionaries` have a significant advantage: superior lookup efficiency for retrieving specific items (`O(1)`). Because `sets` have the same underlying storage mechanism (a hash table) as dictionaries, they have the same characteristics—efficient item lookup.

No matter how large the set grows, item lookup takes about the same time. By contrast, the magnitude of lookup time increases linearly as a function of the list’s size.

Unlike sets, which use hash tables to index objects with hash values (section 3.5.2), lists require traverses to examine whether an item is contained, and the time for such traversing depends directly on the number of the list’s items. This contrast in time complexity highlights the benefit of using sets instead of lists when your business need is item lookup.

Each key in a `dict` object and each item in a `set` object has a corresponding hash value.

---

When objects are unhashable, they can’t serve as dict keys or set items.

```python
failed_dict = {[0, 2]: "even"}
# ERROR: TypeError: unhashable type: 'list'
 
failed_set = {{"a": 0}}
# ERROR: TypeError: unhashable type: 'dict'
```

The `TypeError` exception is raised because we’re trying to use unhashable objects as dictionary keys or set items.

- `A hash function should be so computationally robust that it produces different hash values for different objects`. In rare cases, a hash function can produce the same hash value for different objects—a phenomenon termed `hash collision`, which must be handled according to a specified protocol.
- `A hash function should be so consistent that the same objects always have the same hash values`. When you set a password in an application, the password is hashed by the hasher and stored in a database. When you try to log in again, the entered password string would be hashed and compared with the stored hash value. In these two cases, the same password should produce an identical hash value.
- `For more complicated hashers, hashing is one-way traffic`. By design (such as using a random number), it’s almost impossible to reverse-calculate the raw data based on a hash value. This irreversibility is required where cybersecurity is concerned. Even if hackers get a password’s hash value, they can’t figure out the password from the hash value (at least, not easily). 

String & integer are `hashable`. `list`, dict, set are `unhashable`. The reason is simple: these unhashable data types are mutable. By design, the hash function generates a hash value based on the content of an object.

The content of mutable data can change after creation. If we magically make a list hashable, when we update the list with the changed content, we expect to have a different hash value. But a hash function should consistently produce the same hash value for the same object, and in this case, we expect the hash value to stay the same for the list object. Apparently, the list’s content change, resulting in a hash-value change, is irreconcilable with the expected consistent hash value for the same list object

> all immutable data types are hashable.

- hashable, immutable: int, float, str, tuple, bool, NoneType
- unhashable, mutable: dict, list, set

strings are also immutable in Python. The indication is that it’s impossible to change a character or a substring in a string.

```python
text = "Hello, World."
 
text[-1] = "!"
# ERROR: TypeError: 'str' object does not support item assignment
```

If you need to replace a substring, don’t forget strings’ `replace` method, which creates a new string, as shown in the following code:

```python
text.replace(".", "!")
# output: 'Hello, World!'
```

---

Like their math counterparts, however, set objects in Python have a series of convenient methods for checking relationships between set objects.

Thường mình sẽ convert `list` => `set` rồi dùng những methods này.

---

We often need to check whether the data container has the specific item under examination, a functionality that is termed `membership checking`.

Although lists support membership testing, you should consider using sets if your application is concerned with membership.  
Python requires all the items in a set to be unique because under the hood, sets are implemented by means of a hash table, which offers a significant benefit of constant item lookup time, known as O(1) time complexity. By contrast, membership testing lookup time is linear with the length of the list because Python needs to traverse the sequence to find a potential match. The more items a list has, the more time the traverse costs. Thus, you should use sets when your application is concerned with membership testing.

## List & Tuple

- Similarity:
  * ordered items; objects are accessible through indexing
  * Allow duplicate values
- Tuple:
  * is immutable; cannot change, add or remove items after the tuple has been created; cannot reassign item
- `List`:
  * is mutable; we can append new items to the end of a list, insert items into the middle, change the items, and remove items.

- Tuples are written with square brackets `()`
- Lists are written with square brackets `[]`

Trong Python, có một syntax gọi là `Tuple Unpacking` gần giống như object destructuring trong javascript.

- List is slower than tuple, consumes more memory (due to overhead for change management).
- Tuples are more memory-efficient than lists. When a list and a tuple hold the same data, the list has a larger size than the tuple. 

---

tuples’ immutability doesn’t prevent you from changing their items’ data. If a tuple contains lists, such as `numbers = ([1, 2], [1, 2])`, it’s valid to change the inner lists, such as adding an item to the first list (`numbers[0] .append(3)`). This operation is valid because although we change the content of the inner object, the reference to the object stays the same.

### Sorting

`list` items có ordered nên mình sẽ có thể sort. Nếu list chứa items như string, number thì dễ sort. Nếu list chứa `dict` thì more complicated.

The default sorting order is ascending. If you specify the `reverse` parameter as `True`, you’ll get the list in descending order.

The `list.sort()` method sorts a `list` object in place, which means that sort changes the original list object. `list.sort()` returns `None`.

`sorted()` returns a new sorted list

Another difference is that the `list.sort()` method is only defined for `lists`. In contrast, the `sorted()` function accepts any iterable (tuples, sets, dicts). You can specify a custom sorting function `key` for sorted, too.

---

Using a built-in function as the sorting key

Besides `reverse`, the `sort` method has a `key` parameter. As indicated by its name, this parameter provides a key to the sorting problem. Specifically, you should set `key` with **a function**, which produces a value from each item in the list. These derived values are used for comparison, and the derived order determines the order of the list’s items.

Not only the `sort` method has the `key` parameter. Some other functions, such as `max` and `min`, have the key parameter too. What you learn here can be applied to these functions.

**Each item of the list** is sent to the `key` function. It’s important to note that this `key` function must take exactly one parameter, which corresponds to each item of the list object.

Remember that the function to be set to the key argument should take exactly one parameter.

### Named Tuple

By design, namedtuple is intended to be a lightweight alternative to a full class.

Unlike custom classes, whose instances have per-instance `dict` representations through `__dict__`, named tuples don’t have the underlying dict representations, which makes named tuples a lightweight data model with negligible memory costs. Named tuples can save significant amounts of memory when you need to create thousands of instances.

Each instance of a custom class consumes more memory than an instance of named tuples. When our project evolves, we want our data model to do more things; we’ll move the lightweight data model (named tuple) to a fully equipped custom class.

Compared with built-in types (such as lists, tuples, and dictionaries) and custom classes, named tuples are a more proper, lightweight data model if your business concern is a model to hold data with mostly read-only access requirements. The popular data science Python library pandas, for example, allows you to access each row of its `DataFrame` data model as a named tuple.

---

Named tuple's items have names associated with them. Unlike regular tuples, whose items are accessible by indices, named tuples support dot notation, accessing items just like accessing attributes of a custom class instance.

```python
from collections import namedtuple
 
Task = namedtuple('Task', 'title desc urgency')
task_nt = Task('Laundry', 'Wash clothes', 3)
 
assert task_nt.title == 'Laundry'
assert task_nt.desc == 'Wash clothes'
```

The `namedtuple` is a factory function in the `collections` module. Because it’s a factory function, calling it returns a new class or a new instance object. In this case, we got the `Task` class. 

In the `namedtuple` function, we specified the class name and its attributes for the class. Notably, the data model’s attributes can be set as either a single string (with spaces or commas as separators) or a list object

```python
Task = namedtuple('Task', 'title, desc, urgency')

Task = namedtuple('Task', ['title', 'desc', 'urgency'])
```

Note: A named tuple is a tuple object, so it’s immutable, and changing its stored data directly is not allowed.

## Dealing with Sequence Data

One shared characteristic of lists and tuples is that the held items have a specific **order**. These two data structures are examples of the more general data type `sequence`. Python has other sequence data types, such as strings and bytes.

When we retrieve a subsequence of a list object, we can use slicing. The simplest form of slicing is `list[start:end]`, and the items between the start and end indices (the item at the end index is excluded) are retrieved:

```python
fruits = ["apple", "orange", "banana", "strawberry"]
assert fruits[1:3] == ["orange", "banana"]`
```

By default, the start index is zero, so if you want to retrieve the first n items, the Pythonic way is by omitting the start index and using `list[:end]`.  
By default, the end index is the length of the list, and slicing selection doesn’t include the end index, so if you want to retrieve the last n items of a list, you use `list[start:]`. As you can tell, ignoring the start or end index removes the unnecessary code and improves readability:

```python
assert fruits[:3] == ["apple", "orange", "banana"]
 
assert fruits[1:] == ["orange", "banana", "strawberry"]
```

### Unpack a Sequence, Tuple Unpacking

Unpacking short sequences with one-to-one correspondence

When we work with tuples that contain a few items and need to use all items, we use one-to-one unpacking, in which each item is assigned to a matching variable:

```python
task = (1001, "Laundry", 5)
task_id, task_title, task_urgency = task

print(task_id, task_title, task_urgency)
# output: 1001 Laundry 5

user_data = ("python_user", 35, "male")
username, age, gender = user_data
print(username, age, gender)
# output: python_user 35 male
```

---

Retrieving consecutive items using the starred expression

In the preceding section, we retrieved multiple items by using the one-to-one unpacking technique, which works well with tuples that contain a few items. When the tuples have more items, we may want to retrieve some items as separate variables and some consecutive items as a single variable.

```python
player_scores = [6.1, 6.5, 6.8, 7.1, 7.3, 7.6, 8.2, 8.9]

lowest2, *middles2, highest2 = player_scores
final2 = sum(middles2) / len(middles2)
 
assert lowest0 == lowest2 == player_scores[0]
assert middles0 == middles2 == player_scores[1:-1]
assert highest0 == highest2 == player_scores[-1]
```

A starred expression produces a `list` object of the captured items, regardless of the data type of the original sequence. We can observe this effect with a str object, as shown in the following code snippet. Don’t make the mistake of assuming that the variable b is a str object consisting of all the characters in the middle: 

```python
a, *b, c = "abcdefg"
assert b == ['b', 'c', 'd', 'e', 'f']
```

`The number of captured items in the list object can be zero`. If all items are unpacked with the proper number of variables, leaving zero items to account for, the starred expression produces an empty list. Observe this effect: 

`One assignment can use only one starred expression.` Trying to use two starred expressions is a syntax error. The reason is simple: a starred expression is intended to capture all items that are not accounted for, so when two starred expressions are used, it’s impossible to determine which one should capture which items: 

---

Denoting unwanted items with underscores to remove distraction

`task_id, _, _, task_status = task`

- You can use as many underscores as applicable.
- The underscores are valid variable names.
- You can combine an asterisk and underscore in the starred expression. The following code snippet shows an example:

```python
task = (1001, "Laundry", "Wash clothes", "completed") 
task_id, *_, task_status = task
```

---

You can use parentheses to create layers during unpacking.

```python
list = [1, (2, 3), 4]

a, (b, c), d = list
print(a, b, c, d) # 1 2 3 4
```

## Loops, Iterables and iterations

Iterables: string, list, set, dict, tuple, all sequence data types.

`Iterators` are a special data type from which we can retrieve each of their elements via a process known as `iteration`.

We create an iterator from an iterable by using `iter()` function. Iterators are designed to perform iteration of an iterable’s elements.  
In our code, we rarely need to create an iterator ourselves. Instead, Python does the heavy lifting for us behind the scenes.

`Iterability` refers to the characteristic of an object being an iterable, such that it can be converted to an iterator for iteration.

All iterable can be used inside a `for` loop.

---

Instead of using literal instantiation, `list`, `tuple`, dict, and set constructors can take an iterable to create a corresponding object.

```python
integers_list = list(range(10))
assert integers_list == [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
 
integers_tuple = tuple(integers_list)
assert integers_tuple == (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
 
dict_items = [("zero", 0), ("one", 1), ("two", 2)]
integers_dict = dict(dict_items)
assert integers_dict == {'zero': 0, 'one': 1, 'two': 2}
 
even_numbers = (-2, 4, 0, 2, 4, 2)
unique_evens = set(even_numbers)
assert unique_evens == {0, 2, 4, -2}
```

In the below code example, the map function applies the built-in float function (it’s the float constructor, to be precise) to each string, and the list constructor takes the created map iterator to create a list object of floating-point numbers.

```python
numbers_str = ["1.23", "4.56", "7.89"]
 
numbers_float = list(map(float, numbers_str))
 
assert numbers_float == [1.23, 4.56, 7.89]
```

---

The `zip` function joins the id_numbers and titles side by side, forming a zip iterator that renders elements consisting of one item from each iterable.

```python
id_numbers = [101, 102, 103]
titles = ["Laundry", "Homework", "Soccer"]

zipped_tasks = dict(zip(id_numbers, titles))
assert zipped_tasks == {101: "Laundry", 102: "Homework", 103: "Soccer"}
```

The zip function joins two or more iterables, with each iterable contributing one item to the zip iterator’s elements. Most of the time, you use two iterables in a zip function, which mimics the action of your real-world jacket’s zipper. Thus, if you’re confused about what the zip function does, think about what your jacket’s zipper does: joins two rows of teeth, with the rows alternating to form a pair.

You may know that zipping is a file-compression concept. In Python, the `zipfile` module provides the related functionalities of zipping and unzipping files

### list, dictionary, and set Comprehensions

`list comprehension` is a concise way of creating list objects.

```python
numbers = [1, 2, 3, 4]
squares = [x * x for x in numbers]

assert squares == [1, 4, 9, 16]
```

list comprehension doesn’t look like literals, as it doesn’t list the items directly, but it doesn’t look like the constructor approach either, as it doesn’t call `list()`.

The general syntax is: `[expression for item in iterable]`, in which the expression is a specific operation using each item of the iterable. 

```python
from collections import namedtuple
 
Task = namedtuple("Task", "title, description, urgency") 
tasks = [
    Task("Homework", "Physics and math", 5),
    Task("Laundry", "Wash clothes", 3),
    Task("Museum", "Egypt exhibit", 4)
]

titles = [task.title for task in tasks] # list comprehension
assert titles == ['Homework', 'Laundry', 'Museum']
```

list comprehension is easier to read than using `map()` & lambda function.

---

we can create `dict` objects by using comprehension

The general syntax is `{expr_key: expr_value for item in iterable}`

```python
title_dict0 = {}
# using for loop
for task in tasks:
    title_dict0[task.title] = task.description
# using comprehension
title_dict1 = {task.title: task.description for task in tasks}
 
assert title_dict0 == title_dict1
```

## Functions

Every Python function returns a value, although sometimes implicitly (return `None`). When we define a function that doesn’t return anything, it is still evaluated to return `None`.

---

Function in python can return multiple values (technically it returns a `tuple`).

```python
from statistics import mean, stdev

def generate_stats(measures):
   measure_mean = mean(measures)
   measure_std = stdev(measures)
   return measure_mean, measure_std
```

although it appears that we’re returning two values in the function definition. These two values are packed into a single tuple object. In other words, strictly speaking, when we appear to return multiple values in a function definition, we’re returning a single variable that is a tuple object consisting of these values. Please note that as discussed regarding tuple unpacking (section 4.4), parentheses are optional for creating a tuple object.

You can apply the tuple unpacking technique to using the multiple values returned from a function, which is a concise, Pythonic way to access the individual items of the returned tuple object, as shown in the next listing.

```python
m_mean, m_std = generate_stats(measures)

print(f"Mean: {m_mean}; SD: {m_std}")
# output: Mean: 5.6; SD: 0.8786353054595518
```

Unlike Python, a Java method can only return a single value (or `void`). Java is a strictly typed language and its method signatures are defined by a single return type (e.g., `public int myMethod()`)

---

Nên viết `docstring` cho functions trong python, giống jsdoc, javadoc

### Type hints

Proper type hints tell users what kinds of arguments our functions take and what value our functions return, making our functions more understandable.

Python is `dynamically typed`—the type of variables can change after their creation.

Even though Python is a dynamically typed language, we can provide type hints to the variables that we create in Python. This feature, known as `type hinting`, was added to Python 3.6. To provide a type hint, you use a semicolon after the variable name, after which you specify the type of the variable. Following are some examples:

```python
number: int = 3

name: str = "John"

primes: list = [1, 2, 3]
```

It’s important to know that type hinting doesn’t make Python a statically typed language and that it doesn’t enforce the typing of the variable. You can still assign a value of a different type to a variable that you create with type hinting and run the of code without problems.

---

Using type hints in a function

The syntax `-> data_type` is used to indicate the type for the return value.

```python
from statistics import mean, stdev

def generate_stats(measures: list) -> tuple:
   measure_mean = mean(measures)
   measure_std = stdev(measures)
   return measure_mean, measure_std
```

---

Using arguments with default values

I’ve covered how to set a default value for a parameter in a function definition. When this feature is combined with type hints, all we need to know is the order of the sequence: type hint first and then the default value. The following code snippet shows an example:

```python
def calculate_product(a: int, b: int, multiplier: int = 1) -> int:
   c = a * b * multiplier
   return c
```

The parameter `multiplier` has a default value of `1` with the `int` type. Please note that the spaces used in specifying the parameter’s default value and type are necessary because they help improve the readability of the code. Specifically, you should have spaces before and after the type and the `=` sign.

---

Working with custom classes

When our project grows, we introduce new classes to manage the data. These classes are new types, and we can use them as we do built-in data types such as `int`, `tuple`, and `dict`. The following listing shows how to include custom classes in function definitions by using type hints.

```python
from collections import namedtuple
 
Task = namedtuple("Task", "title description urgency")
 
class User:
    pass # Uses the pass statement as a placeholder
 
def assign_task(pending_task: Task, user: User):
    pass
```

We define two classes: `Task` (using the named tuple technique) and `User` (using a typical class definition). When these classes are defined, we can use them immediately. Python knows these classes are types and that they can be used to indicate the types of the arguments in a function definition.

The `pass` statement is used where code is required to fulfill syntactical requirements. As a placeholder, the `pass` statement does nothing. In the body of a class definition, we’re required to write code to implement the class. In this case, however, we can use pass to validate the class definition.

---

Working with container objects

We have learned that several built-in data types, such as list and tuple, are containers because they can hold other objects. When it comes to type hints for these containers, you may notice that providing a type for the container itself isn’t always meaningful enough.

```python
def complete_tasks_hinted(tasks: list[Task]):
   for task in tasks:
       pass
```

Instead of using only `list`, you can use a pair of brackets following `list` to include the expected data type of the contained objects. In our case, we expect the list object to contain `Task` objects but not str objects. With this change, you’ll notice that the IDE can give you a warning when you use a list object of an incompatible data type, such as strings

---

Taking multiple data types

```python
from statistics import mean, stdev

def generate_stats(measures: list[float] | tuple[float, ...]) 
  -> tuple[float, float]:
   measure_mean = mean(measures)
   measure_std = stdev(measures)
   return measure_mean, measure_std
```

### increase function flexibility with `*args` and `**kwargs`

Knowing positional and keyword arguments

You may have noticed that when we call functions, in the parentheses, we sometimes use the arguments directly, and at other times, we use identifiers preceding the specified arguments. We have different terms for these two types of arguments.

- When the arguments have associated identifiers, they’re `keyword arguments`, and these identifiers are used in the function body to refer to these arguments.  
- When the arguments don’t have associated identifiers, they’re `positional arguments`. In other words, Python processes these arguments based on the arguments’ positions according to the sequence in the function definition.

To understand the distinction between keyword and positional arguments, consider a simple function:

```python
def multiply_numbers(a, b):
   return a * b

# Positional
multiply_numbers(1,2) # a = 1, b = 2
multiply_numbers(2,1) # a = 2, b = 1

# Keyword
multiply_numbers(a=3, b=4)
multiply_numbers(b=4, a=3)

# Positional and Keyword
multiply_numbers(5, b=6) # a = 5, b = 6
multiply_numbers(b=6, 5) # SyntaxError
```

For a typical function like `multiply_numbers`, we can set the parameters as either positional or keyword arguments. There are a few ways to call this function with two parameters.

- Key points regarding the use of positional and keyword arguments:
  * When you use `positional arguments`, the order of these arguments matters. The arguments will be matched with the original parameters in the function head.
  * When you use `keyword arguments`, the order of these arguments doesn't matter. The arguments will be used according to the supplied keywords/identifiers.
  * When you use **both** positional and keyword arguments, you have to place positional arguments **before** any keyword arguments. Otherwise, you’ll raise a `SyntaxError`. 

---

Positional-only and keyword-only arguments

There are two more advanced ways to specify how the arguments should be set: `positional-only arguments` can be set only positionally, and `keyword-only arguments` can be set only with identifiers. If you recall, the sort method has the following head: `sort(*, key=None, reverse=False)`. The `*` specifies that all the arguments behind it should be set only as keyword-only arguments.

By reinforcing keyword-only arguments, you’re forcing readers to use keyword arguments, so they know exactly what parameters they’re setting. You can use this feature if you want some arguments to be set only as keyword arguments.

For positional-only arguments, look at the sum function: `sum(iterable, /, start=0)`. The `/` specifies that the arguments **before** it should be set only as positional arguments. This feature can be useful, but in your code, you rarely need to set arguments that can be used only as positional arguments.

---

How to define a function that accepts a variable number of positional arguments.

```python
def stringify(*items): # all arguments go into one tuple
   print(f"got {items} in {type(items)}")
   return [str(item) for item in items]

def stringify_a(item0, *items): # the remaining go to items tuple
   print(item0, items)

def stringify_b(*items, item0): # Invalid, Python doesn’t know where to stop
   print(item0, items)
```

---

How to define a function that accepts a variable number of keyword arguments

`**kwargs` (keyword arguments) allows a function to accept any number of named arguments that weren't defined in its parameter list. It collects these arguments into a dictionary, where the keys are the argument names and the values are the argument values.

```python
def greet_user(**kwargs):
    print(kwargs)

greet_user(name="Alice", age=30, city="New York")
# Output: {'name': 'Alice', 'age': 30, 'city': 'New York'}
```

We know that the variable number of positional arguments is packed as a `tuple` object. In a similar fashion, the variable number of keyword arguments is packed into a single object: a `dict`. 

```python
def create_report(name, **grades):
   print(f"got {grades} in {type(grades)}")
   report_items = [f"***** Report Begin for {name} *****"]
   for subject, grade in grades.items():
       report_items.append(f"### {subject}: {grade}")
   report_items.append(f"***** Report End for {name} *****")
   print("\n".join(report_items))

create_report("John", math=100, phys=98, bio=95)
# output the following lines:
got {'math': 100, 'phys': 98, 'bio': 95} in <class 'dict'>
***** Report Begin for John *****
### math: 100
### phys: 98
### bio: 95
***** Report End for John *****
```

When you use `**kwargs` in a function, you should remember the syntax rule that `**kwargs` should be placed after all the other parameters. Related to this rule, positional arguments should be placed before all the keyword arguments. 

In general, positional arguments should always precede keyword arguments. `*args` should be the last positional argument, and `**kwargs` should be the last keyword argument.

```python
def example(arg0, arg1, *args, kwarg0, kwarg1, **kwargs):
    pass
```

---

Although using `*args` and `**kwargs` helps improve the flexibility of the defined functions, it’s less explicit to the function’s users regarding the applicable parameters. Thus, we shouldn’t abuse this feature. Only when you can’t know how many positional or keyword arguments you expect the function to accept should you consider using *args and **kwargs. In general, it’s preferred to use explicitly named positional and keyword arguments in a function definition, because these argument names clearly indicate what the parameters are presumed to be doing.

### Lambda Functions

anonymous functions = lambda function/expression. This name is derived from the lambda calculus in mathematics.

Phải dùng `lambda` keyword thay vì `def` như named functions.

`lambda args: expression`

Don’t forget that you need to append a colon to the arguments. When the lambda function contains no arguments, the colon is still required before you specify the expression.

Unlike regular functions, which may return an object, lambda functions don’t return anything. When they do, you get a syntax error:

```python
lambda  x: return x * 2
 
# ERROR: SyntaxError: invalid syntax
```

lambdas use expressions as opposed to statements, and `return x * 2` is a kind of statement.

```python
tasks = [
   {'title': 'Laundry', 'desc': 'Wash clothes', 'urgency': 3},
   {'title': 'Homework', 'desc': 'Physics + Math', 'urgency': 5},
   {'title': 'Museum', 'desc': 'Egyptian things', 'urgency': 2}
]

tasks.sort(key=lambda x: x['urgency'], reverse=True)
```

This lambda function takes one parameter, which stands for each dict object of the list object.

### Functions as objects

everything is an object in Python. Python treats functions like objects too.

- You can store functions in data container (dictionary)
- You can pass function as argument
- You can use function as return value from methods

### Checking functions’ Performance with Decorators

`Decorators` are **functions** that provide additional functionalities to the `decorated functions`. It’s important to note that decorators don’t change the way the decorated functions work; thus, we call this process `decoration`.

```python
import random
import time

def logging_time(func):
   def logger(*args, **kwargs):
       print(f"--- {func.__name__} starts")
       start_t = time.time()
       value_returned = func(*args, **kwargs)
       end_t = time.time()
       print(f"*** {func.__name__} ends; used time: {end_t - start_t:.2f} s")
       return value_returned
   return logger

@logging_time
def example_func2():
   random_delay = random.randint(3, 5) * 0.1
   time.sleep(random_delay)

example_func2()
# output the following two lines:
--- example_func2 starts
*** example_func2 ends; used time: 0.40 s
```

The function `example_func2()` will be decorated by the decorator function `logging_time`. We can apply this decorator function to as many functions as we like, as in this example:

```python
@logging_time
def example_func3():
   pass

@logging_time
def example_func4():
   pass

@logging_time
def example_func5():
   pass
```

The decorator function to multiple functions to perform the shared functionalities.

In essence, a decorator processes a function (take the function in as input), and we call this process `decoration`.

decoration is a process of creating a closure by sending an existing function to the decorator

---

A decorator is a kind of higher-order function; a decorator is a closure-generated function. Decoration is a process of creating a closure by sending an existing function to the decorator.

---

Every Python function returns a value either implicitly as `None` or as an explicitly returned value. Thus, when we define the inner function, we shouldn’t forget to add the `return` statement. Specifically, the return value should be the one that you get by calling the decorated function.

---


While Python Decorators and Java Annotations look very similar (both use the `@` symbol above a function or method), they are fundamentally different in how they work.

In short: Java Annotations are metadata (labels), while Python Decorators are active code (wrappers).

### Use generator functions as a memory-efficient data provider

As a special kind of iterator, a `generator` is created from a generator function. Because a generator is an iterator, it can render its items one by one. A generator is special because it doesn’t store its items, and it retrieves and renders its items when needed. This characteristic means that it’s a memory-efficient iterator for data rendering. 

```python
def perfect_squares(limit):
    n = 1
    while n <= limit:
        yield n * n
        n += 1
 
squares_gen = perfect_squares(upper_limit)
 
sum_gen = sum(squares_gen)
 
assert sum_gen == sum_list == 333333833333500000
```

The `perfect_squares` function is a `generator function`. By calling this function with `upper_limit`, we’re creating a `generator` named `squares_gen`. This generator renders perfect squares: $1^2, 2^2, 3^2, 4^2, ...$ until $1,000,000^2$.

The most significant feature to observe is the `yield` keyword, which is the hallmark of a generator function. Whenever the operation executes to the yield line, it provides the item `n * n`. The coolest thing about a generator is the fact that **it remembers which item it should yield next**.

## Python Modules

k

## Processing & Formatting Strings

Python use f-string: `f"string {variable}"`

f-string supports `list`, `tuple`

Curly braces are the characters for string interpolation in f-strings. Therefore, to escape curly braces inside f-string, you use an extra curly brace: `{{` means `{`, and `}}` means `}`.

### Format Specifier

f-strings allow us to set a `format specifier` (beginning with a colon) to apply additional formatting configurations to the expression in the curly braces.  
As an optional component, the format specifier defines how the interpolated string of the expression should be formatted. 

---

**Text alignment** in f-strings involves three characters: `<`, `>`, and `^`, which align the text left, right, and center, respectively.

To specify text alignment as the format specifier, we use the syntax `f”{expr:x<n}”`, in which `expr` means the interpolated expression, `x` means the padding character (when omitted, it defaults to spaces) for alignment, `<` means left alignment, and `n` is an integer that the string expands in width.

### Convert strings to retrieve the represented data

Python không cho phép compare string vs number. Input từ user là string không phải number. Phải cast về number trước khi xử lý tiếp tục.

Strings that represent floats won’t pass the isnumeric check.

`assert "3.5".isnumeric() == False`

Strings that represent negative integers won’t pass the `isnumeric` check.

---

You can also cast string into `list`, `tuple` & `dict` using the built-in `eval` function, which takes a string as though you typed it in the console and returns the evaluated result.

### Join and Split Strings

When you have multiple string literals, you can join them if they’re separated by whitespaces, such as spaces, tabs, and newline characters.

```python
style_settings = "font-size=large, " "font=Arial, " "color=black, " 
 "align=center"

print(style_settings)
# output: font-size=large, font=Arial, color=black, align=center
```

---

python `list` do not have the `join` method. The `join` method is inside `string`

```python
style_settings = ["font-size=large", "font=Arial", "color=black",  
 "align=center"]
merged_style = ", ".join(style_settings)

print(merged_style)
# output: font-size=large, font=Arial, color=black, align=center
```

---

```python
str.split(separator, maxsplit)
str.rsplit(separator, maxsplit)
```

- `.split()`: Starts scanning the string from the left (start) and moves right.
- `.rsplit()`: Starts scanning the string from the right (end) and moves left.
- If you don't provide a `maxsplit` argument, both methods behave **exactly the same**.

## Exception Handle

The standard way to handle exceptions in Python is to use the `try...except...` block. Many other languages use `try...catch...` blocks.

```python
def process_task_string1(text):
   title, urgency_str = text.split(",")
   try:
       urgency = int(urgency_str)
   except:
       print("Couldn't cast the number")
       return None
   task = Task(title, urgency)
   return task
```

we don’t want to fill the try clause with lots of code because it makes it hard to know which code can lead to an exception. The try clause should include only the code that can raise an exception.

## Class in Python

Python supports multiple inheritance, and Java does not.

Python does not have the `extends` keyword.

We use a custom class instead of a named tuple-based data model here. A custom class gives us the flexibility of changing the instance object’s attributes, which we can’t do with a named tuple mode

### `__init__()`

The constructor is the `__init__` function that you define.

The `__init__()` method is the most essential method that you almost always define in a custom class.

`def __init__(self):`

`self` refers to the instance objects in the method definitions.

Python creates the instance object by calling `__new__` and sends it to `__init__` as the self argument. 

The instance construction is a two-step process that calls `__new__` and `__init__`.

`self` is not a keyword like `def`, `for`, `class`, `lambda`. We’re not required to use self as the parameter name for `__init__()`. We can use any legitimate variable name (but it can’t be a keyword).

```python
class Task:
   def __init__(this):
       print("An instance is created with this instead of self.")

task = Task()
# output: An instance is created with this instead of self.
```

```python
class Task:
   def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       
task = Task("Laundry", "Wash clothes", 3)
```

---

we should specify all the instance attributes in `__init__`, even though some attributes are to be updated through a specific method call. In these cases, these attributes should have a reasonable initial value. The next listing shows the desired pattern.

```python
class Task:
   def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       self.status = "created"
       self.tags = []

   def complete(self):
       self.status = "completed"

   def add_tag(self, tag):
       self.tags.append(tag)
```

### Defining class attributes outside the `__init__` method

The initialization method should provide initialization for an instance object by defining its attributes on a per-instance basis. Notably, there can be shared attributes for all instance objects. In this case, you should not include them as instance attributes and should consider class attributes instead. 

- `Class attributes` are those attributes that belong to the class (as an object), and all the instance objects of the class share the attributes through the class (save memory).
- `instance attribute`

Python does NOT have a `static` keyword like Java or C++.  
In Python, "static" behavior is the default for any variable defined directly inside the class body but outside of any methods. These are called Class Attributes.

```python
class Task:
   # "static" attribute
   user = "the logged in user"
 
   def __init__(self, title, desc, urgency):
       pass
```

### Define Instance, static, and class methods

An `instance method` is intended to be called on an instance object of the class. Thus, when you want to change the data of an individual instance object or run operations that rely on an individual instance object’s data, such as attributes or other instance methods, you need to define instance methods.

The hallmark of an instance method is that you set `self` as its **first parameter**. `self` refers to the instance object in the `__init__` method, which is true for all instance methods.

Under the hood, an instance method is invoked by the class calling the method with the instance as an argument.

```python
class Task:
   def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       self._status = "created"
       
   # instance method
   def complete(self):
       print(f"Memory Address (self): {id(self)}")
       self.status = "completed"


task = Task("Laundry", "Wash clothes", 3)
task.complete()
# output: Memory Address (self): 140508514865536

task_id = f"Memory Address (task): {id(task)}"
print(task_id)
# output: Memory Address (task): 140508514865536
```

---

Unlike an instance method, which uses self as its first parameter, a `static method` doesn’t use self, as it’s intended to be independent of any instance object, and there is no need to refer to a specific instance. To define a static method, we use the `staticmethod` decorator for the function within the body of the class.

```python
from datetime import datetime

class Task:
   @staticmethod
   def get_timestamp():
       now = datetime.now()
       timestamp = now.strftime("%b %d %Y, %H:%M")
       return timestamp

refresh_time = f"Data Refreshed: {Task.get_timestamp()}"

print(refresh_time)
# output: Data Refreshed: Mar 04 2022, 15:43
```

`get_timestamp` is a static method defined with the `@staticmethod` decorator. In this static method, we create a formatted timestamp string, which we can use whenever we need to show users the exact time. To call this method, we use the following pattern: `CustomClass.static_method(arg0, arg1, arg2)`.

---

static methods are utility methods without the need to access individual instance objects. It’s possible that some methods may need to access the attributes of the class. In this case, you need to define a `class method`.

The first hallmark of a class method is that you use `cls` as its first parameter. Like `self` in an instance method, cls is not a keyword, and you can give this argument other applicable names, but it’s a convention to name it cls, and every Python programmer should respect this convention.

The implementation of static methods requires the `staticmethod` decorator. A class method also uses the `classmethod` decorator—the second hallmark of a class method. The method is called a class method because it needs to access the attributes or methods of the class.

```python
class Task:
    def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       self._status = "created"

   @classmethod
   def task_from_dict(cls, task_dict):
       title = task_dict["title"]
       desc = task_dict["desc"]
       urgency = task_dict["urgency"]
       task_obj = cls(title, desc, urgency)
       return task_obj
       
task = Task.task_from_dict(task_dict)

print(task.__dict__)
# output: {'title': 'Laundry', 'desc': 'Wash clothes',
 'urgency': 3, 'status': 'created', 'tags': []}
```

we define a class method called `task_from_dict` with `@classmethod`. In the body of this method, because cls stands for the class that we’re working with (`Task`), we can use the class’s constructor directly—`cls(title, desc, urgency)`—to create an instance object. With this class method, we can conveniently create a Task instance object from a dict object

From a general perspective, a class method is used mostly as a `factory method`, meaning that this kind of method is used to create an instance object from a particular form of data.

### Apply access control to a class

The implication of encapsulation in OOP is that you apply finer access control to the class. You expose only attributes and methods that users need to access and nothing more.

```python
class Task:
   def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       self._status = "created"
       self.note = ""

   def complete(self, note = ""):
       self.status = "completed"
       self.note = self.format_note(note)
     
   def format_note(self, note):
       formatted_note = note.title()
       return formatted_note
```

User should not be able to call `format_note()` directly. The method is intended to be used internally only.

No, Python does not have a `protected` keyword, nor does it have `private` or `public` keywords.  
Python has no formal mechanism that restricts access to any attribute or method. In other words, everything in a class is public.

- The convention in creating an access-control mechanism is to use underscores as the prefix for the attribute or method:
  * A one-underscore prefix means protected
  * and a double-underscore prefix means `private`
- The same mechanism applies to creating protected and private attributes.

However, `__init__` is obviously not a private method.

---

How to access a private method if you need to. You may want to manipulate some code within a package developed by others, for example. As shown in the following code snippet, you can access the private method by calling `_Task__format_note("a note")`:

```python
task._Task__format_note("a note")
# output: 'A Note'
```

This technique is called `name mangling`, which converts a private method to a differently named method, allowing a private method to be called outside the class. Specifically, the name mangling follows the rule `__private_method` -> `_ClassName__private_method`. Thus, `__format_note` becomes `_Task__format_note`, and we can call this private method outside the `Task` class.

using the double underscores as the prefix triggers name mangling.

Python does name mangling automatically when you prefix an attribute name with two underscores (`__`). When an attribute begins with `__`, Python changes the name of the attribute by adding an underscore and the class name to the beginning of the attribute name. For example, `.__name` in a Robot class becomes `._Robot__name`.

Python does not apply name magling to dunder methods like `__init__()`

### Creating read-only Attributes with the property decorator

```python
class Task:
   def __init__(self, title, desc, urgency):
       self.title = title
       self.desc = desc
       self.urgency = urgency
       self._status = "created"

   @property
   def status(self):
       return self._status
       
   def complete(self):
       self._status = "completed"

task = Task("Laundry", "Wash clothes", 3)

print(task.status)
# output: created
```

- The instance has a protected attribute `_status`.
- We define an instance method `status`, which is decorated by the property decorator.
- In the complete method, we update the `_status` attribute. 

For encapsulation purposes, we don’t allow users to set the status attribute freely. To update a task’s status to completed, for example, they should call the complete method

The `@property` decorator makes a method accessible as though it’s an attribute. For simplicity, you can refer to a method with the property decorator as a property, and you don’t need to use parentheses `()` call operator to access a property.

### customize string representation for a class

k

## Inheritance

```python
class Employee:
   def __init__(self, name, employee_id):
       self.name = name
       self.employee_id = employee_id

   def login(self):
       print(f"An employee {self.name} just logged in.")

   def logout(self):
       print(f"An employee {self.name} just logged out.")


class Supervisor(Employee):
   pass
```

When you define a subclass, you specify the superclass in parentheses following the class’s name. Here, the superclass is `Employee`, so we place it after `Supervisor`.

---

Python does not have `@Override` annotation.

```python
class Supervisor(Employee):
   def login(self):
       print(f"A supervisor {self.name} just logged in.")
```

Python supports multiple inheritance—a class inherits from multiple classes

---

```python
class Employee:
   def __init__(self, name, employee_id):
       self.name = name
       self.employee_id = employee_id

   # protected
   def _request_vacation(self):
       print("Send a vacation request to the employee's supervisor.")

   # private
   def __transfer_group(self):
       print("Transfer the employee to a different group.")

class Supervisor(Employee):
   def do_something(self):
       self._request_vacation()
       self.__transfer_group()

supervisor = Supervisor("John", "1001")
supervisor.do_something()
# output the following lines:
Send a vacation request to the employee's supervisor.
# ERROR: AttributeError: 'Supervisor' object has no attribute 
 '_Supervisor__transfer_group'
```

subclasses inherit protected methods; subclass do not inherit private methods

## File and Directory Access

The `Path` class inherits from `PurePath`. `PurePath` defines all the methods which don't directly interact with the file system, e.g. splitting a path into stem and extension etc. The `Path` class defines additional methods like `cwd()` (getting the current working directory) which actually interact with the file system.
