## 10.1. Operating System Interface

The [`os`](https://docs.python.org/3/library/os.html#module-os) module provides dozens of functions for interacting with the operating system:

\>>>

```
>>> import os
>>> os.getcwd()      # Return the current working directory
'C:\\Python38'
>>> os.chdir('/server/accesslogs')   # Change current working directory
>>> os.system('mkdir today')   # Run the command mkdir in the system shell
0
```

Be sure to use the `import os` style instead of `from os import *`. This will keep [`os.open()`](https://docs.python.org/3/library/os.html#os.open) from shadowing the built-in [`open()`](https://docs.python.org/3/library/functions.html#open) function which operates much differently.

The built-in [`dir()`](https://docs.python.org/3/library/functions.html#dir) and [`help()`](https://docs.python.org/3/library/functions.html#help) functions are useful as interactive aids for working with large modules like [`os`](https://docs.python.org/3/library/os.html#module-os):

\>>>

```
>>> import os
>>> dir(os)
<returns a list of all module functions>
>>> help(os)
<returns an extensive manual page created from the module's docstrings>
```

For daily file and directory management tasks, the [`shutil`](https://docs.python.org/3/library/shutil.html#module-shutil) module provides a higher level interface that is easier to use:

\>>>

```
>>> import shutil
>>> shutil.copyfile('data.db', 'archive.db')
'archive.db'
>>> shutil.move('/build/executables', 'installdir')
'installdir'
```



## 10.2. File Wildcards

The [`glob`](https://docs.python.org/3/library/glob.html#module-glob) module provides a function for making file lists from directory wildcard searches:

\>>>

```
>>> import glob
>>> glob.glob('*.py')
['primes.py', 'random.py', 'quote.py']
```



## 10.3. Command Line Arguments

Common utility scripts often need to process command line arguments. These arguments are stored in the [`sys`](https://docs.python.org/3/library/sys.html#module-sys) module’s *argv* attribute as a list. For instance the following output results from running `python demo.py one two three` at the command line:

\>>>

```
>>> import sys
>>> print(sys.argv)
['demo.py', 'one', 'two', 'three']
```

The [`argparse`](https://docs.python.org/3/library/argparse.html#module-argparse) module provides a more sophisticated mechanism to process command line arguments. The following script extracts one or more filenames and an optional number of lines to be displayed:

```
import argparse

parser = argparse.ArgumentParser(prog = 'top',
    description = 'Show top lines from each file')
parser.add_argument('filenames', nargs='+')
parser.add_argument('-l', '--lines', type=int, default=10)
args = parser.parse_args()
print(args)
```

When run at the command line with `python top.py --lines=5 alpha.txt beta.txt`, the script sets `args.lines` to `5` and `args.filenames` to `['alpha.txt', 'beta.txt']`.