# Глава 1. Основы языка Python

Читать эту главу стоит последовательно, так как важные аспекты языка буду расскрываться последовательно, базируюясь на предыдущих абзацах.

[ToC]

## Стандартные типы данных

Like most languages, Python has a number of basic types including integers, floats, booleans, and strings. These data types behave in ways that are familiar from other programming languages.

Как и в большинстве языков, в Python существуют набор базовых типов данных: целые числа (**integers**), числа с плавающей точкой (**floats**), логический тип данных (**booleans**) и строки (**strings**). 

Поведение этих типов данных достаточно схоже с многими другими языками программирования.

Эти типы данных ведут себя так, как это известно на других языках программирования.

###Числа (numbers)

Целые числа (**integers**) и числа с плавающей точкой (**floats**) ведут себя также как и в других языках:

```python
x = 3
print(type(x)) # Выводит "<class 'int'>"
print(x)       # Выводит "3"
print(x + 1)   # Сложение (addition); выводит "4"
print(x - 1)   # Вычитание (subtraction); выводит "2"
print(x * 2)   # Умножение (multiplication); выводит "6"
print(x ** 2)  # Возведение в степень (exponentiation); выводит "9"
x += 1
print(x)  # Выводит "4"
x *= 2
print(x)  # Выводит "8"
y = 2.5
print(type(y)) # Выводит "<class 'float'>"
print(y, y + 1, y * 2, y ** 2) # Выводит "2.5 3.5 5.0 6.25"
```

Заметьте что в отличие от многих языков, в Python нет унарных операторов инкрементирования (`x++`) и уменьшения (`x--`).

В Python также присутствует встроенная поддержка комплексных чисел, более подробную информацию вы сможете найти [в документации](https://docs.python.org/3.5/library/stdtypes.html#numeric-types-int-float-complex).

###Логические типы (booleans)

В Python представлены все стандартные буленовские операторы, однако вместо привычных для си-подобных языков символов (`&&`, `||`, и т.д.) используются английские слова:

```python
t = True
f = False
print(type(t)) # Выводит "<class 'bool'>"
print(t and f) # Логическое И; выводит "False"
print(t or f)  # Логическое ИЛИ; выводит "True"
print(not t)   # Логическое ОТРИЦАНИЕ; выводит "False"
print(t != f)  # Логическая операция XOR; выводит "True"
```

### Строки (strings)

Python предлгает отличные инструменты для работы со строками:

```python
hello = 'hello'    # Строковые литералы обозначаются одинарными кавычками
world = "world"    # или двойными; на усмотрение программиста.
print(hello)       # Выводит "hello"
print(len(hello))  # Длина строки; выводит "5"
hw = hello + ' ' + world  # Конкатенация строк (склейка)
print(hw)  # выводит "hello world"
hw12 = '%s %s %d' % (hello, world, 12)  # форматирование строки аналогично sprintf из СИ
print(hw12)  # выводит "hello world 12"
```

String objects have a bunch of useful methods; for example:

```python
s = "hello"
print(s.capitalize())  # Capitalize a string; prints "Hello"
print(s.upper())       # Convert a string to uppercase; prints "HELLO"
print(s.rjust(7))      # Right-justify a string, padding with spaces; prints "  hello"
print(s.center(7))     # Center a string, padding with spaces; prints " hello "
print(s.replace('l', '(ell)'))  # Replace all instances of one substring with another;
                                # prints "he(ell)(ell)o"
print('  world '.strip())  # Strip leading and trailing whitespace; prints "world"
```

You can find a list of all string methods [in the documentation](https://docs.python.org/3.5/library/stdtypes.html#string-methods).

## Циклы (loops)

### while

Of course, we can use Python for more complicated tasks than adding two and two together. For instance, we can write an initial sub-sequence of the [Fibonacci series](https://en.wikipedia.org/wiki/Fibonacci_number) as follows:

```
>>> # Fibonacci series:
... # the sum of two elements defines the next
... a, b = 0, 1
>>> while a < 10:
...     print(a)
...     a, b = b, a+b
...
0
1
1
2
3
5
8
```

This example introduces several new features.

- The first line contains a *multiple assignment*: the variables `a` and `b` simultaneously get the new values 0 and 1. On the last line this is used again, demonstrating that the expressions on the right-hand side are all evaluated first before any of the assignments take place. The right-hand side expressions are evaluated from the left to the right.

- The [`while`](https://docs.python.org/3/reference/compound_stmts.html#while) loop executes as long as the condition (here: `a < 10`) remains true. In Python, like in C, any non-zero integer value is true; zero is false. The condition may also be a string or list value, in fact any sequence; anything with a non-zero length is true, empty sequences are false. The test used in the example is a simple comparison. The standard comparison operators are written the same as in C: `<` (less than), `>` (greater than), `==` (equal to), `<=` (less than or equal to), `>=` (greater than or equal to) and `!=` (not equal to).

- The *body* of the loop is *indented*: indentation is Python’s way of grouping statements. At the interactive prompt, you have to type a tab or space(s) for each indented line. In practice you will prepare more complicated input for Python with a text editor; all decent text editors have an auto-indent facility. When a compound statement is entered interactively, it must be followed by a blank line to indicate completion (since the parser cannot guess when you have typed the last line). Note that each line within a basic block must be indented by the same amount.

- The [`print()`](https://docs.python.org/3/library/functions.html#print) function writes the value of the argument(s) it is given. It differs from just writing the expression you want to write (as we did earlier in the calculator examples) in the way it handles multiple arguments, floating point quantities, and strings. Strings are printed without quotes, and a space is inserted between items, so you can format things nicely, like this:

  \>>>

  ```
  >>> i = 256*256
  >>> print('The value of i is', i)
  The value of i is 65536
  ```

  The keyword argument *end* can be used to avoid the newline after the output, or end the output with a different string:

  \>>>

  ```
  >>> a, b = 0, 1
  >>> while a < 1000:
  ...     print(a, end=',')
  ...     a, b = b, a+b
  ...
  0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
  ```

### for

The [`for`](https://docs.python.org/3/reference/compound_stmts.html#for) statement in Python differs a bit from what you may be used to in C or Pascal. Rather than always iterating over an arithmetic progression of numbers (like in Pascal), or giving the user the ability to define both the iteration step and halting condition (as C), Python’s `for` statement iterates over the items of any sequence (a list or a string), in the order that they appear in the sequence. For example (no pun intended):

```
>>> # Measure some strings:
... words = ['cat', 'window', 'defenestrate']
>>> for w in words:
...     print(w, len(w))
...
cat 3
window 6
defenestrate 12
```

Code that modifies a collection while iterating over that same collection can be tricky to get right. Instead, it is usually more straight-forward to loop over a copy of the collection or to create a new collection:

```
# Strategy:  Iterate over a copy
for user, status in users.copy().items():
    if status == 'inactive':
        del users[user]

# Strategy:  Create a new collection
active_users = {}
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```

## The [`range()`](https://docs.python.org/3/library/stdtypes.html#range) Function

If you do need to iterate over a sequence of numbers, the built-in function [`range()`](https://docs.python.org/3/library/stdtypes.html#range) comes in handy. It generates arithmetic progressions:

```
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4
```

The given end point is never part of the generated sequence; `range(10)` generates 10 values, the legal indices for items of a sequence of length 10. It is possible to let the range start at another number, or to specify a different increment (even negative; sometimes this is called the ‘step’):

```
range(5, 10)
   5, 6, 7, 8, 9

range(0, 10, 3)
   0, 3, 6, 9

range(-10, -100, -30)
  -10, -40, -70
```

To iterate over the indices of a sequence, you can combine [`range()`](https://docs.python.org/3/library/stdtypes.html#range) and [`len()`](https://docs.python.org/3/library/functions.html#len) as follows:

\>>>

```
>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
...     print(i, a[i])
...
0 Mary
1 had
2 a
3 little
4 lamb
```

In most such cases, however, it is convenient to use the [`enumerate()`](https://docs.python.org/3/library/functions.html#enumerate) function, see [Looping Techniques](https://docs.python.org/3/tutorial/datastructures.html#tut-loopidioms).

A strange thing happens if you just print a range:

\>>>

```
>>> print(range(10))
range(0, 10)
```

In many ways the object returned by [`range()`](https://docs.python.org/3/library/stdtypes.html#range) behaves as if it is a list, but in fact it isn’t. It is an object which returns the successive items of the desired sequence when you iterate over it, but it doesn’t really make the list, thus saving space.

We say such an object is [iterable](https://docs.python.org/3/glossary.html#term-iterable), that is, suitable as a target for functions and constructs that expect something from which they can obtain successive items until the supply is exhausted. We have seen that the [`for`](https://docs.python.org/3/reference/compound_stmts.html#for) statement is such a construct, while an example of function that takes an iterable is [`sum()`](https://docs.python.org/3/library/functions.html#sum):

\>>>

```
>>> sum(range(4))  # 0 + 1 + 2 + 3
6
```

Later we will see more functions that return iterables and take iterables as arguments. Lastly, maybe you are curious about how to get a list from a range. Here is the solution:

\>>>

```
>>> list(range(4))
[0, 1, 2, 3]
```

In chapter [Data Structures](https://docs.python.org/3/tutorial/datastructures.html#tut-structures), we will discuss in more detail about [`list()`](https://docs.python.org/3/library/stdtypes.html#list).

## `break` and `continue` Statements, and `else` Clauses on Loops

The [`break`](https://docs.python.org/3/reference/simple_stmts.html#break) statement, like in C, breaks out of the innermost enclosing [`for`](https://docs.python.org/3/reference/compound_stmts.html#for) or [`while`](https://docs.python.org/3/reference/compound_stmts.html#while) loop.

Loop statements may have an `else` clause; it is executed when the loop terminates through exhaustion of the iterable (with [`for`](https://docs.python.org/3/reference/compound_stmts.html#for)) or when the condition becomes false (with [`while`](https://docs.python.org/3/reference/compound_stmts.html#while)), but not when the loop is terminated by a [`break`](https://docs.python.org/3/reference/simple_stmts.html#break) statement. This is exemplified by the following loop, which searches for prime numbers:

\>>>

```
>>> for n in range(2, 10):
...     for x in range(2, n):
...         if n % x == 0:
...             print(n, 'equals', x, '*', n//x)
...             break
...     else:
...         # loop fell through without finding a factor
...         print(n, 'is a prime number')
...
2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

(Yes, this is the correct code. Look closely: the `else` clause belongs to the [`for`](https://docs.python.org/3/reference/compound_stmts.html#for) loop, **not** the [`if`](https://docs.python.org/3/reference/compound_stmts.html#if) statement.)

When used with a loop, the `else` clause has more in common with the `else` clause of a [`try`](https://docs.python.org/3/reference/compound_stmts.html#try) statement than it does with that of [`if`](https://docs.python.org/3/reference/compound_stmts.html#if) statements: a [`try`](https://docs.python.org/3/reference/compound_stmts.html#try) statement’s `else` clause runs when no exception occurs, and a loop’s `else` clause runs when no `break` occurs. For more on the `try` statement and exceptions, see [Handling Exceptions](https://docs.python.org/3/tutorial/errors.html#tut-handling).

The [`continue`](https://docs.python.org/3/reference/simple_stmts.html#continue) statement, also borrowed from C, continues with the next iteration of the loop:

\>>>

```
>>> for num in range(2, 10):
...     if num % 2 == 0:
...         print("Found an even number", num)
...         continue
...     print("Found a number", num)
Found an even number 2
Found a number 3
Found an even number 4
Found a number 5
Found an even number 6
Found a number 7
Found an even number 8
Found a number 9
```

## Условия (conditionals)

Perhaps the most well-known statement type is the [`if`](https://docs.python.org/3/reference/compound_stmts.html#if) statement. For example:

```
>>> x = int(input("Please enter an integer: "))
Please enter an integer: 42
>>> if x < 0:
...     x = 0
...     print('Negative changed to zero')
... elif x == 0:
...     print('Zero')
... elif x == 1:
...     print('Single')
... else:
...     print('More')
...
More
```

There can be zero or more [`elif`](https://docs.python.org/3/reference/compound_stmts.html#elif) parts, and the [`else`](https://docs.python.org/3/reference/compound_stmts.html#else) part is optional. The keyword ‘`elif`’ is short for ‘else if’, and is useful to avoid excessive indentation. An `if` … `elif` … `elif` … sequence is a substitute for the `switch` or `case` statements found in other languages.

## Контейнеры

Python includes several built-in container types: lists, dictionaries, sets, and tuples.

### Списки (lists)

A list is the Python equivalent of an array, but is resizeable and can contain elements of different types:

```python
xs = [3, 1, 2]    # Create a list
print(xs, xs[2])  # Prints "[3, 1, 2] 2"
print(xs[-1])     # Negative indices count from the end of the list; prints "2"
xs[2] = 'foo'     # Lists can contain elements of different types
print(xs)         # Prints "[3, 1, 'foo']"
xs.append('bar')  # Add a new element to the end of the list
print(xs)         # Prints "[3, 1, 'foo', 'bar']"
x = xs.pop()      # Remove and return the last element of the list
print(x, xs)      # Prints "bar [3, 1, 'foo']"
```

As usual, you can find all the gory details about lists [in the documentation](https://docs.python.org/3.5/tutorial/datastructures.html#more-on-lists).

**Slicing:** In addition to accessing list elements one at a time, Python provides concise syntax to access sublists; this is known as *slicing*:

```python
nums = list(range(5))     # range is a built-in function that creates a list of integers
print(nums)               # Prints "[0, 1, 2, 3, 4]"
print(nums[2:4])          # Get a slice from index 2 to 4 (exclusive); prints "[2, 3]"
print(nums[2:])           # Get a slice from index 2 to the end; prints "[2, 3, 4]"
print(nums[:2])           # Get a slice from the start to index 2 (exclusive); prints "[0, 1]"
print(nums[:])            # Get a slice of the whole list; prints "[0, 1, 2, 3, 4]"
print(nums[:-1])          # Slice indices can be negative; prints "[0, 1, 2, 3]"
nums[2:4] = [8, 9]        # Assign a new sublist to a slice
print(nums)               # Prints "[0, 1, 8, 9, 4]"
```

We will see slicing again in the context of numpy arrays.

**Loops:** You can loop over the elements of a list like this:

```python
animals = ['cat', 'dog', 'monkey']
for animal in animals:
    print(animal)
# Prints "cat", "dog", "monkey", each on its own line.
```

If you want access to the index of each element within the body of a loop, use the built-in `enumerate` function:

```python
animals = ['cat', 'dog', 'monkey']
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# Prints "#1: cat", "#2: dog", "#3: monkey", each on its own line
```

**List comprehensions:** When programming, frequently we want to transform one type of data into another. As a simple example, consider the following code that computes square numbers:

```python
nums = [0, 1, 2, 3, 4]
squares = []
for x in nums:
    squares.append(x ** 2)
print(squares)   # Prints [0, 1, 4, 9, 16]
```

You can make this code simpler using a **list comprehension**:

```python
nums = [0, 1, 2, 3, 4]
squares = [x ** 2 for x in nums]
print(squares)   # Prints [0, 1, 4, 9, 16]
```

List comprehensions can also contain conditions:

```python
nums = [0, 1, 2, 3, 4]
even_squares = [x ** 2 for x in nums if x % 2 == 0]
print(even_squares)  # Prints "[0, 4, 16]"
```

### Словари (dictionaries)

A dictionary stores (key, value) pairs, similar to a `Map` in Java or an object in Javascript. You can use it like this:

```python
d = {'cat': 'cute', 'dog': 'furry'}  # Create a new dictionary with some data
print(d['cat'])       # Get an entry from a dictionary; prints "cute"
print('cat' in d)     # Check if a dictionary has a given key; prints "True"
d['fish'] = 'wet'     # Set an entry in a dictionary
print(d['fish'])      # Prints "wet"
# print(d['monkey'])  # KeyError: 'monkey' not a key of d
print(d.get('monkey', 'N/A'))  # Get an element with a default; prints "N/A"
print(d.get('fish', 'N/A'))    # Get an element with a default; prints "wet"
del d['fish']         # Remove an element from a dictionary
print(d.get('fish', 'N/A')) # "fish" is no longer a key; prints "N/A"
```

You can find all you need to know about dictionaries [in the documentation](https://docs.python.org/3.5/library/stdtypes.html#dict).

**Loops:** It is easy to iterate over the keys in a dictionary:

```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal in d:
    legs = d[animal]
    print('A %s has %d legs' % (animal, legs))
# Prints "A person has 2 legs", "A cat has 4 legs", "A spider has 8 legs"
```

If you want access to keys and their corresponding values, use the `items` method:

```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.items():
    print('A %s has %d legs' % (animal, legs))
# Prints "A person has 2 legs", "A cat has 4 legs", "A spider has 8 legs"
```

**Dictionary comprehensions:** These are similar to list comprehensions, but allow you to easily construct dictionaries. For example:

```python
nums = [0, 1, 2, 3, 4]
even_num_to_square = {x: x ** 2 for x in nums if x % 2 == 0}
print(even_num_to_square)  # Prints "{0: 0, 2: 4, 4: 16}"
```

### Множества (sets)

A set is an unordered collection of distinct elements. As a simple example, consider the following:

```python
animals = {'cat', 'dog'}
print('cat' in animals)   # Check if an element is in a set; prints "True"
print('fish' in animals)  # prints "False"
animals.add('fish')       # Add an element to a set
print('fish' in animals)  # Prints "True"
print(len(animals))       # Number of elements in a set; prints "3"
animals.add('cat')        # Adding an element that is already in the set does nothing
print(len(animals))       # Prints "3"
animals.remove('cat')     # Remove an element from a set
print(len(animals))       # Prints "2"
```

As usual, everything you want to know about sets can be found [in the documentation](https://docs.python.org/3.5/library/stdtypes.html#set).

**Loops:** Iterating over a set has the same syntax as iterating over a list; however since sets are unordered, you cannot make assumptions about the order in which you visit the elements of the set:

```python
animals = {'cat', 'dog', 'fish'}
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# Prints "#1: fish", "#2: dog", "#3: cat"
```

**Set comprehensions:** Like lists and dictionaries, we can easily construct sets using set comprehensions:

```python
from math import sqrt
nums = {int(sqrt(x)) for x in range(30)}
print(nums)  # Prints "{0, 1, 2, 3, 4, 5}"
```

### Упорядоченные списки (tuples)

A tuple is an (immutable) ordered list of values. A tuple is in many ways similar to a list; one of the most important differences is that tuples can be used as keys in dictionaries and as elements of sets, while lists cannot. Here is a trivial example:

```python
d = {(x, x + 1): x for x in range(10)}  # Create a dictionary with tuple keys
t = (5, 6)        # Create a tuple
print(type(t))    # Prints "<class 'tuple'>"
print(d[t])       # Prints "5"
print(d[(1, 2)])  # Prints "1"
```

[The documentation](https://docs.python.org/3.5/tutorial/datastructures.html#tuples-and-sequences) has more information about tuples.

## Функции (functions)

Python functions are defined using the `def` keyword. For example:

```python
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'

for x in [-1, 0, 1]:
    print(sign(x))
# Prints "negative", "zero", "positive"
```

We will often define functions to take optional keyword arguments, like this:

```python
def hello(name, loud=False):
    if loud:
        print('HELLO, %s!' % name.upper())
    else:
        print('Hello, %s' % name)

hello('Bob') # Prints "Hello, Bob"
hello('Fred', loud=True)  # Prints "HELLO, FRED!"
```

There is a lot more information about Python functions [in the documentation](https://docs.python.org/3.5/tutorial/controlflow.html#defining-functions).

## Классы (classes)

The syntax for defining classes in Python is straightforward:

```python
class Greeter(object):

    # Constructor
    def __init__(self, name):
        self.name = name  # Create an instance variable

    # Instance method
    def greet(self, loud=False):
        if loud:
            print('HELLO, %s!' % self.name.upper())
        else:
            print('Hello, %s' % self.name)

g = Greeter('Fred')  # Construct an instance of the Greeter class
g.greet()            # Call an instance method; prints "Hello, Fred"
g.greet(loud=True)   # Call an instance method; prints "HELLO, FRED!"
```

You can read a lot more about Python classes [in the documentation](https://docs.python.org/3.5/tutorial/classes.html).