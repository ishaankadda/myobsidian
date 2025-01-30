# Types of Modules
1. A **module** can be written in Python itself
2. A **module** can be written in C and loaded dynamically at run-time. example: `re` (the regular expression library in python)
3. A **module** can be built-in. These are available as imports by default in the interpreter

## 1. The Module Search Path
When you execute the statement `import mod`, the python interpreter looks for a file named `mod.py` in a list of directories assembled from the following sources:
1. The directory in which the input script was run (cwd)
2. List of directories contained in the `PYTHONPATH` environment variable, if it is set.
3. An installation-dependent list of directories configured at the time Python is installed.

One a module has been imported, you can access `module.__file__` to  determine the location from where it was found.
```
>>> import mod
>>> mod.__file__
'C:\Users\\john\\mod.py'
```

*A module can **only** be imported when it is located in any one of the directories listed inside `sys.path`.* 

This can be done two ways:
1. Modify the location of `mod.py` to ensure that it is inside one of these directories.
2. Modify `sys.path` during runtime.

## 2. The `import` Statement
**Module** contents are made available to the caller with the `import` statement. There are many forms to it:
### 2.1. `import <module_name>`
This does not make the module contents directly accessible to the caller. Each module has it's own **private symbol table**, which serves as the global symbol table for all the objects defined in the module. This way, each module creates a separate **namespace** as already noted.
After being imported, modules get placed into the **local symbol table.** However, the objects in the module's **private symbol table** are only accessible by prefixing them with the name of the module:
```
>>> s
NameError: name 's' is not defined
>>> mod.s
'If Comrade Napoleon says it, it must be right.'
```

### 2.2. `from <module_name> import <name (s)>`
Allows importing individual objects from the module *directly into the caller's **local symbol table***!
Any objects that already exist with the same name in the local symbol table will be overwritten.

```
from <module_name> import *
```
This imports all objects from `<module_name>` except for any that begin with the underscore (\_) character. (not recommended for large-scale production code, enters too many names into the local symbol table and risks conflicts and overwrites.)

### 2.3. `from <module_name> import <name> as <alt_name>`
imports individual objects from `<module_name>` while entering them into the local symbol table with alternate names. Can be used to avoid conflicts.

### 2.4. `import <module_name> as <alt_name>`
Imports entire module under an alternate name.

### 2.5. `import` from inside a function definition
You can import a module inside a function definition. Until the function is called, the import will not be made. 

This also means that any import errors that were going to happen, will not happen until the function is called, as `sys.path` is dynamic. A `try` statement with an `except ImportError` clause can be used to guard against unsuccessful imports.

## 3. The `dir()` Function
The `dir()` function returns a list of defined names in a namespace. When called with empty arguments, it returns the list of names in the **local symbol table**.

## 4. Importing `*` From a Module
When `__all__` is not defined in a module, it means that when `from <module_name> import *` is called, everything is imported. (as opposed to **package import behaviour**, where not defining `__all__` means that no modules are imported. Refer [[Python Packages#1.1. Importing `*` From a Package]] for the package analogue of `__all__`).