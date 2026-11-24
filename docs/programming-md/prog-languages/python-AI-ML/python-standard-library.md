# Python Standard Library

## Logging

kkk

## pathlib

This module offers classes representing filesystem paths with semantics appropriate for different operating systems. Path classes are divided between `pure paths`, which provide purely computational operations without I/O, and `concrete paths`, which inherit from pure paths but also provide I/O operations.

---

The `pathlib.Path` class inherits from `PurePath`, this class represents concrete paths of the system’s path flavour (instantiating it creates either a `PosixPath` or a WindowsPath).

`PurePath` defines all the methods which don't directly interact with the file system, e.g. splitting a path into stem and extension etc. The `Path` class defines additional methods like `cwd()` (getting the current working directory) which actually interact with the file system.

`Path` instantiates a `concrete path` for the platform the code is running on.

---

`__file__` is a built-in Python attribute that stores the file path of the currently executing script or imported module. It is automatically set by Python's import system to help code locate itself on the filesystem.

### Pure paths

```python
p = PureWindowsPath('c:/foo/bar/setup.py')

p.parents[0]
PureWindowsPath('c:/foo/bar')
p.parents[1]
PureWindowsPath('c:/foo')
p.parents[2]
PureWindowsPath('c:/')
```

---

`PurePath.with_suffix(suffix)` Return a new path with the `suffix` changed. If the original path doesn’t have a suffix, the new `suffix` is appended instead. If the `suffix` argument is an empty string, the original suffix is removed

### Expanding and resolving paths

`Path.resolve()` converts a file or directory path into a clean, `absolute path`. It also resolves any symbolic links (symlinks) and cleans up relative navigation segments like `.` (current directory) or `..` (parent directory).

```python
from pathlib import Path

# Assuming your current working directory is /Users/alex/projects
relative_path = Path("../documents/./notes.txt")

# Resolve to a clean, absolute path
full_path = relative_path.resolve()

print(full_path)
# Output: /Users/alex/documents/notes.txt
```

When you run the command `uv run pdf-converter Ses3.1sum.pdf`, `Path.resolve()` sees a relative path (no leading `/`), it automatically prepends `Path.cwd()` (`/home/anhao/document/pdf-converter`), turning `"Ses3.1sum.pdf"` into: `/home/anhao/document/pdf-converter/Ses3.1sum.pdf`

`Path(__file__).resolve().parents[2] / "rules.md"` is anchored to where `__init__.py` is located on disk (`src/pdf_converter/__init__.py` → up 2 levels → project root), so it always finds the root `rules.md` no matter what directory you run the command from.

---

`Path.expanduser()` return a new path with expanded ~ and ~user constructs, as returned by `os.path.expanduser()`. If a home directory can’t be resolved, RuntimeError is raised.

On Unix and Windows, return the argument with an initial component of `~` or `~user` replaced by that _user_’s home directory.

`expanduser()` replaces a leading ~ or ~user with the user's home directory, while `resolve()` makes a path absolute, normalizes .. segments, and resolves symbolic links.

## argparse

When known to the interpreter, the script name and additional arguments thereafter are turned into a list of strings and assigned to the `argv` variable in the `sys` module. You can access this list by executing `import sys`. The length of the list is at least one; when no script and no arguments are given, `sys.argv[0]` is an empty string.

---

The `argparse` module makes it easy to write user-friendly command-line interfaces. The program defines what arguments it requires, and `argparse` will figure out how to parse those out of `sys.argv`.

The `argparse` module’s support for command-line interfaces is built around an instance of `argparse.ArgumentParser`. It is a container for argument specifications and has options that apply to the parser as whole:

```python
parser = argparse.ArgumentParser(
          prog='ProgramName',
          description='What the program does',
          epilog='Text at the bottom of help'
)
```

---

The `ArgumentParser.parse_args(args=None)` method runs the parser and places the extracted data in a `argparse.Namespace` object.

when `args` is `None`, it automatically reads `sys.argv[1:]` (everything after the script/command name, i.e., `["Ses3.1sum.pdf"]`).

## sys

`argv` (short for **argument vector**) is an optional list of command-line arguments passed into `main()`, typed as `list[str] | None = None`.

`argv[0]` is the script name (it is operating system dependent whether this is a full pathname or not).

