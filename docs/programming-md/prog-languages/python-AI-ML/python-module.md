# Manage Python Project

## Installing Python

[uv](https://docs.astral.sh/uv/) is a Python package and project manager, written in Rust.

A `Python interpreter` is a program that reads and executes Python code. It acts as both a translator and a runtime environment, converting the high-level Python code you write into machine-readable instructions that your computer's processor can understand and execute.  
`CPython`, written in `C`, is the most common and reference implementation of the Python interpreter.

`IPython` (Interactive Python) is an enhanced interactive shell built on top of the standard Python interpreter.

- Python is included by default on almost every Linux and MacOS system, but you might want to use a different version than the default.
- Python isn’t usually included by default on Windows

`uv` python install to `C:\Users\anhao\.local\bin`

add vào PATH trên window cho python

On powershell, run `python.exe --version`

## `uv` Commands

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

Now using `uv`:

```bash
uv init new_app
cd new_app

# Install packages
uv add flask requests
# install OK but stored inside `.venv`, no change to the `pyproject.toml`
uv pip install numpy

# not `python3 main.py`
uv run main.py
```

---

If you’re working on an existing project and want to migrate from a `requirements.txt` file to using uv, then you can run the following command:

```bash
uv add -r requirements.txt
```

This command imports the dependencies declared in your existing requirements.txt file into the uv infrastructure.

---

If you want to start managing an existing project with uv, then navigate to the project directory and run `uv init`.

This command will create the uv project structure for you. It won’t overwrite the main.py file if you have one, but it’ll create the file if it’s missing. It neither modifies your Git repository nor your README.md file.

However, this command won’t work if you already have a `pyproject.toml` file in place. If that’s the case, then you can move the file to another location and run the uv init command. Finally, you can update the new file with any relevant configuration from your old pyproject.toml.

---

```bash
$ uv run main.py
Using CPython 3.13.2
Creating virtual environment at: .venv
Hello from rpcats!
```

The first time you run this command, it’ll display the Python version you’re currently using for the project. It’ll also inform you that uv has created a dedicated Python virtual environment for the project (`.venv`).

The `uv.lock` file uses the TOML format. You should add this file to version control. However, you shouldn’t edit this file manually.

---

Other commands:

```bash
uv python list

uv python install 3.10

uv --help
uv --version
uv self update

# show the dependency tree
uv tree

uv venv

uv sync

# generate a requirements.txt
uv pip freeze > requirements.txt
```

`pip` install packages

`venv` (or virtual env) for creating virtual environment

`uv sync` create the `venv` using the `.lock` file. Nếu rename directory thì phải delete `.venv`, run `uv sync` rồi `source` venv again.

## Project structure

uv supports managing Python projects, which define their dependencies in a `pyproject.toml` file.  
In the old method, nếu xóa `venv` sẽ mất package. Because `uv` store package information in `pyproject.toml & .lock` file, deleting `venv` will not cause any damage.

The `.python-version` file contains the default Python version for the current project. This file tells uv which Python version to use when creating a dedicated virtual environment for the project.

Even though uv has an alternative interface that mimics `pip`, you shouldn’t use it to install dependencies because these commands won’t update the uv.lock file or pyproject.toml automatically like `uv add` does.

---

Python project metadata is defined in a `pyproject.toml` file. uv requires this file to identify the root directory of a project.

## Dependency Management

It is not recommended to modify the project environment manually, e.g., with `uv pip install`. For project dependencies, use `uv add` to add a package to the environment.

`uv add httpx`

Dependencies declared in a `requirements.txt` file can be added to the project with the `-r` option:

```bash
uv add -r requirements.txt
```

---

```bash
uv add --upgrade requests
```

This command upgrades `requests` and updates its version information in the uv.lock file. This action allows you to work with the library’s latest version.

---

remove dependency

```bash
uv remove <package_name>
```

This is the list of packages installed in the project’s virtual environment and their specific versions.

```bash
$ uv pip list
Package            Version
------------------ ---------
certifi            2025.1.31
charset-normalizer 3.4.1
idna               3.10
iniconfig          2.1.0
packaging          24.2
pluggy             1.5.0
pytest             8.3.5
requests           2.32.3
urllib3            2.3.0
```

In the example above, you used the pip interface of uv. In this case, that’s perfectly valid because you aren’t changing the environment but retrieving information from it.

---

In most development environments, you’ll have dependencies that aren’t required for running the code but are vital for developing it. For example, testing libraries like pytest, code formatters like Ruff, and static type checkers like mypy might be some of these development dependencies.

```bash
uv add --dev pytest
```

## Locking and Syncing the Environment

As you’ve already learned, uv uses the `uv.lock` file to lock a project’s dependencies. Locking consists of capturing your project’s specific dependencies in the lockfile. This process makes it possible to reproduce your working environment in all possible configurations, including the Python version and distribution, the operating system, and the architecture.

---

As a counterpart, `syncing` is the process of installing the required packages from the lockfile into the project’s development environment.

Both locking and syncing processes are automatically handled by `uv`. For example, when you execute `uv run`, the project is locked and synced before the command is invoked. This behavior ensures that your project’s environment is always up to date.

If you have an existing project with a `.toml` file (e.g., a clone project from GitHub), you can easily install dependencies by running: `uv sync`

## Virtual Environment

`uv` automatically create a virtual environment when we install package. Còn nếu muốn tự tạo thì

```bash
uv venv
source .venv/bin/activate

# un-source the virtual env
deactivate
```

To run a command in the project environment, use `uv run`. Alternatively the project environment can be activated as normal for a virtual environment.  
When `uv run` is invoked, it will create the project environment if it does not exist yet or ensure it is up-to-date if it exists.

---

activating the environment puts `.venv/bin` at the front of your `$PATH`

Virtual environment activation is purely an in-memory, per-session state. Here is exactly when it stops persisting:

- When you run `deactivate`: It reverts the shell back to your default system state.
- When you switch to another virtual environment: Sourcing a different `.venv/bin/activate` overwrites $PATH and $VIRTUAL_ENV with the new environment's locations.
- When you close the terminal session: Because activation only modifies the memory of that specific shell process, closing the tab/window ends it. A newly opened terminal will always start clean (unactivated).
- It does not leak into other windows: Activating a virtual environment in Terminal Tab A has zero effect on Terminal Tab B, even if they are open at the same time.

---

Check current `venv`

```bash
echo $VIRTUAL_ENV
/Users/anhao/Documents/programming/python/learn-python/learn/.venv

# alternative
which python3
# If activated, the path will point directly inside your .venv/bin/python3
/Users/anhao/Documents/programming/python/learn-python/learn/.venv/bin/python3
# If not activated, it looks something like this
which python3
/opt/homebrew/bin/python3
```

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

## The Python Interpreter

Python is an interpreted language. No compilation and linking is necessary.

### `python` vs `python3` command line utilities

Tại sao lại có 2 commands: `python --version`, `python3 --version`

- The `python` command typically referred to the Python 2.x interpreter.
- The `python3` command specifically referred to the Python 3.x interpreter. This distinction was crucial when both Python 2 and Python 3 were commonly installed on a system, especially on Linux distributions where system utilities might still rely on Python 2.

- In modern environments:
  - Python 2 has reached its end-of-life, and Python 3 is the current and actively developed version.
  - On many newer systems and installations, the `python` command is often aliased or symlinked to `python3`, meaning both commands will invoke the Python 3 interpreter.
  - However, this is not universally true, and it's still possible to encounter systems where python might still point to an older Python 2 installation.

---

Virtual Environment

A best practice among Python developers is to use a project-specific virtual environment.

## Package, Modules

```python
def main():
    print("This program is running directly!")

if __name__ == "__main__":
    main()
```

When run directly (`python script.py`): Python automatically assigns `__name__ = "__main__"`. The `if` condition evaluates to `True`, so `main()` runs.

When imported (`import script` from another file or test suite): Python assigns `__name__` to the file's module name (e.g., `"script"`). The condition evaluates to `False`, so `main()` is skipped.

Without this guard, any top-level executable code runs automatically upon import. Wrapping execution in `if __name__ == "__main__":` allows your file to be safely reusable.

If you write:

```python
# my_module.py
def helper():
    return "ready"

def main():
    print("Pipeline executing!")

main()  # <--- Naked call at file level
########################
# test_suite.py
from my_module import helper
```

The moment Python hits that `from my_module import helper` line, it runs `my_module.py` entirely, encounters `main()`, and prints `"Pipeline executing!"`—even though you only wanted to import `helper`.

Putting `main()` inside `if __name__ == "__main__":` blocks that execution during an import because `__name__` evaluates to `"my_module"` instead of `"__main__"`.

---

- Module: An organizational unit that has its own namespace and contains Python objects. Both single files and directories become module objects upon import.
- Package: Simply a special type of module that has a `__path__` attribute. The presence of `__path__` tells Python's import system: "I can contain submodules or child packages inside me."

```python
import os
import urllib

print(type(os))       # <class 'module'>  (implemented as os.py)
print(type(urllib))   # <class 'module'>  (implemented as urllib/ directory)

print(hasattr(os, "__path__"))      # False -> Not a package
print(hasattr(urllib, "__path__"))  # True  -> It is a package!
```

Python packages are expected to contain one or more Python modules, which are directories containing an `__init__.py`. By default, a single root module is expected at `src/<package_name>/__init__.py`.

```txt
pyproject.toml
src
└── foo
    └── __init__.py
```

The `src/` directory is the default directory for module discovery.

## wheel

A Python wheel is a pre-built binary package format with a .whl file extension. It is the modern standard for distributing Python software.

Under the hood, a wheel is simply a ZIP archive containing ready-to-install code, package metadata, and any pre-compiled binary extensions. When you run `pip install`, pip prefers downloading a wheel so it can extract the files directly into your environment without running a build process.

A Python wheel (.whl) does not contain bytecode.

- Java Archive (`.jar`)
  - Archive Format is `zip` file
  - Contains Compiled `.class` JVM bytecode
  - Compiled before packaging (javac compiles `.java` to `.class`)
  - The JVM reads `.class` bytecode directly from inside the .jar file at runtime without extracting it.
- Python Wheel (.whl)
  - Archive Format is also `zip` file
  - Contains Raw `.py` source files
  - Compiled on the target machine during installation (`pip`/`uv` generates `.pyc` files in `__pycache__/`)
  - Python does not normally execute `.whl` files directly. Instead, uv or pip unzips the wheel into .venv/lib/python3.x/site-packages/, and then compiles the .py files into .pyc bytecode files inside `__pycache__/` folders on disk.

## Build backend

 `uv init` creates a packaged project by default; `--no-package` keeps the project layout flat (a `main.py` at the top level instead of a `src/` package).

## Entry Point

```toml
[project.scripts]
pdf-converter = "pdf_converter:main"
```

`pdf_converter` refers to the Python package at `src/pdf_converter` (specifically `src/pdf_converter/__init__.py`), not the root directory.

## src layout vs flat layout

The “flat layout” refers to organising a project’s files in a folder or repository, such that the various configuration files and import packages are all in the top-level directory.

```txt
.
├── README.md
├── noxfile.py
├── pyproject.toml
├── setup.py
├── awesome_package/
│   ├── __init__.py
│   └── module.py
└── tools/
    ├── generate_awesomeness.py
    └── decrease_world_suck.py
```

The “src layout” deviates from the flat layout by moving the code that is intended to be importable (i.e. import awesome_package, also known as import packages) into a subdirectory. This subdirectory is typically named src/, hence “src layout”.

```txt
.
├── README.md
├── noxfile.py
├── pyproject.toml
├── setup.py
├── src/
│    └── awesome_package/
│       ├── __init__.py
│       └── module.py
└── tools/
    ├── generate_awesomeness.py
    └── decrease_world_suck.py
```

## PVM (Python Virtual Machine)

k
