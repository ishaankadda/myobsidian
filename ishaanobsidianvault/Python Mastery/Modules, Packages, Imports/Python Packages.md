**Packages** allow for a hierarchical structuring of the module namespace using **dot notation**. This helps avoid collisions between module names.

## 0. Importing a Package
Let us assume that we have a Python package containing *only* two files, `mod1.py` and `mod2.py`.
```
| pkg
|-- mod1.py
|-- mod2.py
```
In this case, the **package** `pkg` allows us to isolate the importing of both modules from other variables that are local to the caller, similar to how a namespace works.
```
>>> import pkg.mod1, import pkg.mod2
>>> from pkg.mod2 import Bar as Qux
>>> 
>>> pkg.mod1.foo()
[mod1] foo()
>>>
>>> pkg.mod2.bar()
[mod2] bar()
>>>
>>> x = Qux()
>>> x
<pkg.mod2.Bar object at 0x036DFFD0>

```

**Note: Technically, you could import the modules or their contents into the local namespace using `import pkg`, but this would be of no avail.** It will *not* place any of the modules in `pkg` in the local namespace. For example, calling `pkg.mod1` will return a `AttributeError: module 'pkg' has no attribute 'mod1'`


If a file named `__init__.py` is present in a package directory, it is invoked when the package or a module in the package is imported.

## 1. `__init__.py` and the `__all__` Variable
If a file named `__init__.py` is located inside a package directory, it is called when the package is imported. 
This can be used for initialization of package-level data. For example:

**`pkg/__init__.py`**
```
A = ['quux', 'corge', 'gault']
```

The global list `A` is initialised. In turn, a **module** in the **package** can access the global variable by importing it in turn:

**`pkg/mod1.py`**
```
def foo():
	from pkg import A
	print(A)
```

You can import the modules inside `__init__.py` so that when you import `pkg`, those modules are also imported automatically and can be called using `pkg.mod1`.

### 1.1. Importing `*` From a Package
```
from <pkg_name> import *
```
When the above statement is encountered, Python checks the `__init__.py` file for a **list** named `__all__`, for the list of modules to be imported. Check [[Python Modules#4. Importing `*` From a Module]] for the behaviour of `__all__` when defined and not defined in a **module**.

**`pkg/__init__.py`**
```
__all__ = ['mod1', 'mod2', 'mod3', 'mod3']
```

## 2. Subpackages
Python packages can be nested with subpackages uptil arbitrary depth.

Modules in one subpackage can reference the modules in a **sibling subpackage** using:
1. absolute imports - refer to [Real Python: Python Modules and Packages - An Introduction](https://realpython.com/python-modules-packages/#subpackages)
2. relative imports - refer to [Real Python: Python Modules and Packages - An Introduction](https://realpython.com/python-modules-packages/#subpackages)

