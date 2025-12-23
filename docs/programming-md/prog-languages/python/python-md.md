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

The `sort()` method sorts a `list` object in place, which means that sort changes the original list object. `sort()` returns `None`.

---

tuples’ immutability doesn’t prevent you from changing their items’ data. If a tuple contains lists, such as `numbers = ([1, 2], [1, 2])`, it’s valid to change the inner lists, such as adding an item to the first list (`numbers[0] .append(3)`). This operation is valid because although we change the content of the inner object, the reference to the object stays the same.

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

## Dealing with sequence data

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

## Unpack a Sequence, Tuple Unpacking

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

### list, dictionary, and set comprehensions

`list comprehension` is a concise way of creating list objects.

```python
numbers = [1, 2, 3, 4]
squares = [x * x for x in numbers]
 
assert squares == [1, 4, 9, 16]
```

list comprehension doesn’t look like literals, as it doesn’t list the items directly, but it doesn’t look like the constructor approach either, as it doesn’t call list.

`[expression for item in iterable]`, in which the expression is a specific operation using each item of the iterable. 

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

`**kwargs` (keyword arguments) allows a function to accept any number of named arguments that weren't defined in its parameter list. It collects these arguments into a dictionary, where the keys are the argument names and the values are the argument values.

```python
def greet_user(**kwargs):
    print(kwargs)

greet_user(name="Alice", age=30, city="New York")
# Output: {'name': 'Alice', 'age': 30, 'city': 'New York'}
```

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

### Join and split strings

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

### Regex in Python

k

## Exception Handle

k

## OOP in Python

Python supports multiple inheritance, and Java does not.

Python does not have the `extends` keyword.

The constructor is the `__init__` function that you define.

We use a custom class instead of a named tuple-based data model here. A custom class gives us the flexibility of changing the instance object’s attributes, which we can’t do with a named tuple mode

## File and Directory Access

The `Path` class inherits from `PurePath`. `PurePath` defines all the methods which don't directly interact with the file system, e.g. splitting a path into stem and extension etc. The `Path` class defines additional methods like `cwd()` (getting the current working directory) which actually interact with the file system.
