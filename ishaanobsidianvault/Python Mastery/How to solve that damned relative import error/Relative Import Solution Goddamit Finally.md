# StackOverflow Blog
Here is the link to the StackOverflow blog I finally found the solution in.
https://stackoverflow.com/questions/14132789/relative-imports-for-the-billionth-time

# The Solution (explained in short)
### ye yaad rakh lo!
Jo script directly run hoti hai `python scriptname.py` se, that script has `__name__ = '__main__'` and all other modules and package names are defined relative to the location of that script. For example, if we run `python script1.py`, while importing `mod1` it will have the value of `__name__ = pkg1.mod1` whereas, if we run `python script2.py`, while importing `mod1` it will have the value of `__name__ = module_tut.pkg1.mod1`

In other words, if a relative import manages to supercede the sibling level of the main script that was run, then relative imports will break.

# The Solution (long explanation)
The `__name__` variable of the script being imported must have atleast as many dots as the import statement.
Basically, if you run a python script, you can only make relative imports at or below the level of the script you run.

### Example to explain
```
module_tut
 |-- __init__.py
 |-- pkg1
	 |-- __init__.py
	 |-- mod1.py
 |-- pkg2
	 |-- __init__.py
	 |-- mod2.py
 |-- script1.py
 script2.py
```

**`script2.py`**
```
print(f'inside script2.py, __name__={__name__}')
from module_tut import script1
```

**`script1.py`**
```
print(f'inside script1.py, __name__={__name__}')
from .pkg1 import mod1
print('bruh')
```

**`mod1.py`**
```
print('inside mod1.py. __name__ is:', __name__)
from .. import pkg2
import sys
print('mod1.py is being run. interpreter curent path:', sys.path)
```

**`mod2.py`**
```
print('inside mod2.py. __name__ is:', __name__)
```


When we navigate to `./module_tut` and run `python script2.py`, here is the output:
```
username:~/Desktop/module_tut$ python script1.py
inside script1.py, __name__=__main__
Traceback (most recent call last):
  File "/home/kadda/Desktop/module_tut/script1.py", line 3, in <module>
    from .pkg1 import mod1
ImportError: attempted relative import with no known parent package
```

Why did the relative import error happen and what to do about it? Here's the explanation.


> *"Python adds the current directory to the search path; if it finds the to-be-imported module in the current directory, it does not consider it as a part of a package, and the package information will not become a part of the module name.*
> 
> *Naturally, an absolute import from the current directory will have no package information in its `__name__`."*

The script successfully locates `mod1` from inside `.pkg1` and `import sys` runs successfully. `__name__`, which contains the name of the module being imported, also prints `pkg1.mod1`


