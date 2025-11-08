# Python notes

## Installation

[uv](https://docs.astral.sh/uv/) is a Python package and project manager, written in Rust.

A Python interpreter is a program that reads and executes Python code. It acts as both a translator and a runtime environment, converting the high-level Python code you write into machine-readable instructions that your computer's processor can understand and execute.

`CPython`, written in C, is the most common and reference implementation of the Python interpreter.

- Python is included by default on almost every Linux and MacOS system, but you might want to use a different version than the default.
- Python isn’t usually included by default on WindowsPython isn’t usually included by default on Windows

### python vs python3 command line utilities

Tại sao lại có 2 commands: `python --version`, `python3 --version`

- The `python` command typically referred to the Python 2.x interpreter.
- The `python3` command specifically referred to the Python 3.x interpreter. This distinction was crucial when both Python 2 and Python 3 were commonly installed on a system, especially on Linux distributions where system utilities might rely on Python 2.

- In modern environments:
  * Python 2 has reached its end-of-life, and Python 3 is the current and actively developed version.
  * On many newer systems and installations, the `python` command is often aliased or symlinked to python3, meaning both commands will invoke the Python 3 interpreter.
  * However, this is not universally true, and it's still possible to encounter systems where python might still point to an older Python 2 installation.

## Virtual environment

A best practice among Python developers is to use a project-specific virtual environment.

## `uv` commands

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

