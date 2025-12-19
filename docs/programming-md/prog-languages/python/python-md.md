# Python Notes

## Installation

[uv](https://docs.astral.sh/uv/) is a Python package and project manager, written in Rust.

A Python interpreter is a program that reads and executes Python code. It acts as both a translator and a runtime environment, converting the high-level Python code you write into machine-readable instructions that your computer's processor can understand and execute.

`CPython`, written in C, is the most common and reference implementation of the Python interpreter.

- Python is included by default on almost every Linux and MacOS system, but you might want to use a different version than the default.
- Python isn’t usually included by default on WindowsPython isn’t usually included by default on Windows

`IPython` (Interactive Python) is an enhanced interactive shell built on top of the standard Python interpreter.

### python vs python3 command line utilities

Tại sao lại có 2 commands: `python --version`, `python3 --version`

- The `python` command typically referred to the Python 2.x interpreter.
- The `python3` command specifically referred to the Python 3.x interpreter. This distinction was crucial when both Python 2 and Python 3 were commonly installed on a system, especially on Linux distributions where system utilities might rely on Python 2.

- In modern environments:
  - Python 2 has reached its end-of-life, and Python 3 is the current and actively developed version.
  - On many newer systems and installations, the `python` command is often aliased or symlinked to python3, meaning both commands will invoke the Python 3 interpreter.
  - However, this is not universally true, and it's still possible to encounter systems where python might still point to an older Python 2 installation.

## Virtual Environment

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

## Python Basics

Run `python3` in the terminal to run python in the terminal. Use `<C-d>` or type `exit()` to leave the Python prompt and return to a terminal prompt.

Use `"""` or three backticks to create multi-line comments in Python.

The `"""` is also used to create multi-line string literals.

## Built-in Data Types

### Numeric Types

There are three distinct numeric types: integers, floating-point numbers, and complex numbers (`int`, `float`, `complex`).

### Collections

There are 4 built-in data types in Python used to store collections of data: Tuple, Dictionary, List, Set

**Dictionaries** are used to store data values in `key:value` pairs. A dictionary is a collection which is ordered*, changeable and do not allow duplicates.

- Dictionaries are changeable, meaning that we can change, add or remove items after the dictionary has been created.
- As of Python version 3.7, dictionaries are ordered. In Python 3.6 and earlier, dictionaries are unordered.
- Duplicate keys will overwrite existing values

Written with curly brackets

---

Tuple

- used to store multiple items in a single variable.
- ordered (Tuple items are indexed) and unchangeable
- allow duplicate values
- Written with square brackets `()`

cannot change, add or remove items after the tuple has been created.

Trong Python, có một syntax gọi là **Tuple Unpacking** gần giống như object destructuring trong javascript.

---

List

Lists can be changed after they are created, while tuples cannot.

Written with square brackets `[]`

List is slower than tuple, consumes more memory (due to overhead for change management).

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

---

- The single leading underscore (`_`) before a name in Python is a naming convention that signifies to other developers that the name is intended for internal use.
- `__init__` Denotes special built-in methods recognized by the language (e.g., constructor, iterator).
- A single underscore (`_`) is used as a variable name when you need to unpack a value but intend to ignore or throw away that value.

Trong nodeJs cũng có kiểu naming convention tương tự.

## Modules

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

## File and Directory Access

The `Path` class inherits from `PurePath`. `PurePath` defines all the methods which don't directly interact with the file system, e.g. splitting a path into stem and extension etc. The `Path` class defines additional methods like `cwd()` (getting the current working directory) which actually interact with the file system.
