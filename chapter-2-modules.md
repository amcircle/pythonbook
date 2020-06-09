# Глава 2. Модули

Объявленные функции и переменные при выходе из интерпретатора Python и повторном входе будут потеряны. Поэтому, если вы хотите написать более длинную программу, вам лучше использовать текстовый редактор, чтобы подготовить ввод для интерпретатора и запустить его с этим файлом в качестве ввода. Это известно как создание *скрипта*. По мере увеличения объема программы, вы можете разделить ее на несколько файлов. Вы также можете использовать удобную функцию, написанную в нескольких программах, без копирования ее определения в каждую программу.

Для поддержки этого в Python есть способ поместить определения в файл и использовать их в скрипте или в интерактивном экземпляре интерпретатора. Такой файл называется *модуль*; определения из модуля могут быть *импортированы* в другие модули или в *основной* модуль (набор переменных, к которым у вас есть доступ в скрипте, выполняемом на верхнем уровне и в режиме калькулятора).

Модуль - это файл, содержащий определения и операторы Python. Имя файла - это имя модуля с добавленнием `.py`. Внутри модуля имя модуля (в виде строки) доступно как значение глобальной переменной `__name__`. Например, используйте ваш любимый текстовый редактор, чтобы создать файл с именем `fibo.py` в текущем каталоге со следующим содержимым:

```
# Модуль чисел Фибоначчи

def fib(n):    # вывод ряда Фибоначчи до n
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

def fib2(n):   # возвращает ряд Фибоначчи до n
    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a+b
    return result
```

Теперь войдите в интерпретатор Python и импортируйте этот модуль с помощью следующей команды:

\>>>

```
>>> import fibo
```

Это не вводит имена функций, определенных в `fibo`, непосредственно в текущей таблице символов; он вводит только имя модуля `fibo`. Используя имя модуля, вы можете получить доступ к функциям:

\>>>

```
>>> fibo.fib(1000)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__
'fibo'
```

Если вы собираетесь использовать функцию часто, вы можете присвоить ее локальному имени:

\>>>

```
>>> fib = fibo.fib
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```



## 2.1. Подробнее о модулях

Модуль может содержать исполняемые операторы, а также определения функций. Эти операторы предназначены для инициализации модуля. Эти операторы предназначены для инициализации модуля. Они выполняются только *в первый раз*, когда имя модуля встречается в операторе импорта.

Каждый модуль имеет свою приватную таблицу символов, которая используется в качестве глобальной таблицы символов всеми функциями, которые определены в модуле. Таким образом, автор модуля может использовать глобальные переменные в модуле, не беспокоясь о случайных столкновениях с глобальными переменными пользователя. С другой стороны, если вы знаете, что делаете, вы можете обратиться к глобальным переменным модуля аналогично тому, как ссылаетесь на его функции, `modname.itemname`.

Модули могут импортировать другие модули. Обычно, но не обязательно размещать все операторы [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) в начале модуля (или, в данном случае, скрипта). Имена импортированных модулей помещаются в глобальную таблицу символов импортирующего модуля.

Существует вариант оператора [`import`](https://docs.python.org/3/reference/simple_stmts.html#import), который импортирует имена из модуля непосредственно в таблицу символов импортирующего модуля. Например:

\>>>

```
>>> from fibo import fib, fib2
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Это не помещает имя модуля, из которого импортируются данные, в локальную таблицу символов (поэтому в примере `fibo` не определен).

Существует вариант для импорта всех имен, определенных в модуле:

\>>>

```
>>> from fibo import *
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Это импортирует все имена, кроме тех, которые начинаются с подчеркивания (`_`). В большинстве случаев программисты Python не используют эту возможность, поскольку она вводит неизвестный набор имен в интерпретатор, возможно, скрывая некоторые имена, которые вы уже определили.

Обратите внимание, что в целом практика импорта `*` из модуля или пакета не одобряется, так как это часто создает плохо читаемый код.

Если за именем модуля следует `as`, то имя, следующее за ` as`, напрямую связано с импортированным модулем.

\>>>

```
>>> import fibo as fib
>>> fib.fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Это импортирует модуль так же, как это делает `import fibo`, с той лишь разницей, что он доступен как `fib`.

Его также можно использовать с [`from`](https://docs.python.org/3/reference/simple_stmts.html#from) с тем же эффектом:

\>>>

```
>>> from fibo import fib as fibonacci
>>> fibonacci(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

Note

Из соображений эффективности каждый модуль импортируется только один раз за сеанс интерпретатора. Поэтому, если вы меняете свои модули, вы должны перезапустить интерпретатор – или, если это только один модуль, который вы хотите протестировать в интерактивном режиме, используйте [`importlib.reload()`](https://docs.python.org/3/library/importlib.html#importlib.reload), например, `import importlib; importlib.reload(modulename)`.

### 2.1.1. Выполнение модулей в виде скриптов

Когда вы запускаете модуль Python с

```
python fibo.py <arguments>
```

код в модуле будет выполнен так же, как если бы вы его импортировали, но с  `__name__` определенным как `"__main__"`.

```
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
```

Вы можете сделать файл доступным для использования в качестве скрипта, а также в качестве импортируемого модуля, потому что код, который анализирует командную строку, запускается, только если модуль выполняется как «основной» файл:

```
$ python fibo.py 50
0 1 1 2 3 5 8 13 21 34
```

Если модуль просто импортирован, код не выполнится:

\>>>

```
>>> import fibo
>>>
```

Это часто используется либо для обеспечения удобного пользовательского интерфейса для модуля, либо для тестирования (запуск модуля в качестве сценария выполняет набор тестов).

### 2.1.2. Путь поиска модулей

Когда импортируется модуль `spam` интерпретатор сначала ищет встроенный модуль с таким именем. Если он не найден, он ищет файл с именем `spam.py` в списке каталогов, заданных переменной [`sys.path`](https://docs.python.org/3/library/sys.html#sys.path). [`sys.path`](https://docs.python.org/3/library/sys.html#sys.path) инициализируется из:

- Каталога, содержащего входной скрипт (или текущего каталога, если файл не указан).
- [`PYTHONPATH`](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH) (список имен каталогов, с тем же синтаксисом, что и у переменной оболочки `PATH`).
- Зависит от установки по умолчанию.

Note

В файловых системах, которые поддерживают символические ссылки, каталог, содержащий входной скрипт, рассчитывается после использования символической ссылки. Другими словами, каталог, содержащий символическую ссылку,  **не** добавляется к пути поиска модуля.

После инициализации программы на Python могут изменять [`sys.path`](https://docs.python.org/3/library/sys.html#sys.path) . Каталог, содержащий выполняемый скрипт, помещается в начало пути поиска, перед стандартным путем к библиотеке. Это означает, что скрипты в этом каталоге будут загружены вместо модулей с теми же именами в каталоге библиотеки. Это ошибка, если замена не предусмотрена. См. Раздел [Standard Modules](https://docs.python.org/3/tutorial/modules.html#tut-standardmodules) для получения дополнительной информации.

### 2.1.3. “Скомпилированные” файлы Python

Чтобы ускорить загрузку модулей, Python кэширует скомпилированную версию каждого модуля в каталоге `__pycache__` под именем `module.*version*.pyc`, где версия кодирует формат скомпилированного файла; обычно он содержит номер версии Python. Например, в выпуске CPython 3.3 скомпилированная версия spam.py будет кэшироваться как `__pycache__/spam.cpython-33.pyc` . Это соглашение об именах позволяет сосуществовать скомпилированным модулям из разных релизов и разных версий Python.

Python проверяет дату модификации источника по скомпилированной версии, чтобы определить, устарела ли она и нуждается ли в перекомпиляции. Это полностью автоматический процесс. Кроме того, скомпилированные модули не зависят от платформы, поэтому одна и та же библиотека может совместно использоваться системами с разными архитектурами.



## 2.2. Стандартные модули

В Python есть библиотека стандартных модулей, описанных в отдельном документе, Справочнике по библиотеке Python» (далее «Справочник по библиотеке»). Некоторые модули встроены в интерпретатор; они обеспечивают доступ к операциям, которые не являются частью ядра языка, но тем не менее являются встроенными, либо для эффективности, либо для обеспечения доступа к примитивам операционной системы, таким как системные вызовы. Набор таких модулей является опцией конфигурации, которая также зависит от базовой платформы. Например, модуль [`winreg`](https://docs.python.org/3/library/winreg.html#module-winreg) предоставляется только в системах Windows.

Особого внимания заслуживает модуль [`sys`](https://docs.python.org/3/library/sys.html#module-sys), который встроен в каждый интерпретатор Python. Переменные `sys.ps1` и ` sys.ps2` определяют строки, используемые в качестве основного и вторичного приглашений:

\>>>

```
>>> import sys
>>> sys.ps1
'>>> '
>>> sys.ps2
'... '
>>> sys.ps1 = 'C> '
C> print('Yuck!')
Yuck!
C>
```

Эти две переменные определяются только в интерактивном режиме интерпретатора.

Переменная `sys.path` представляет собой список строк, который определяет путь поиска интерпретатора для модулей. Он инициализируется путем по умолчанию, взятым из переменной среды  [`PYTHONPATH`](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH), или из встроенного значения по умолчанию, если [`PYTHONPATH`](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH) не задано. Вы можете изменить его, используя стандартные операции со списком:

\>>>

```
>>> import sys
>>> sys.path.append('/ufs/guido/lib/python')
```



## 2.3. Функция [`dir()`](https://docs.python.org/3/library/functions.html#dir)

The built-in function [`dir()`](https://docs.python.org/3/library/functions.html#dir) is used to find out which names a module defines. It returns a sorted list of strings:

Встроенная функция [`dir()`](https://docs.python.org/3/library/functions.html#dir) используется для определения имен, которые определяются в модуле и возвращает отсортированный список строк:

\>>>

```
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
>>> dir(sys)  
['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
 '__package__', '__stderr__', '__stdin__', '__stdout__',
 '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
 '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
 'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
 'call_tracing', 'callstats', 'copyright', 'displayhook',
 'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
 'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
 'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
 'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
 'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
 'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
 'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
 'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
 'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
 'thread_info', 'version', 'version_info', 'warnoptions']
```

Функция [`dir()`](https://docs.python.org/3/library/functions.html#dir) без аргументов возвращает имена, определенные на текущий момент:

\>>>

```
>>> a = [1, 2, 3, 4, 5]
>>> import fibo
>>> fib = fibo.fib
>>> dir()
['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

Обратите внимание, что перечислены все типы имен: переменные, модули, функции и т. д.

Однако, список из встроенных функций и переменных определен в стандартном модуле  [`builtins`](https://docs.python.org/3/library/builtins.html#module-builtins):

\>>>

```
>>> import builtins
>>> dir(builtins)  
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException',
 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning',
 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError',
 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning',
 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False',
 'FileExistsError', 'FileNotFoundError', 'FloatingPointError',
 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError',
 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError',
 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError',
 'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented',
 'NotImplementedError', 'OSError', 'OverflowError',
 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError',
 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning',
 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError',
 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError',
 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError',
 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning',
 'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__',
 '__debug__', '__doc__', '__import__', '__name__', '__package__', 'abs',
 'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable',
 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits',
 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit',
 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr',
 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass',
 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview',
 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property',
 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice',
 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars',
 'zip']
```



## 2.4. Пакеты

Пакеты - это способ структурировать пространство имен модулей Python с помощью «точечных имен модулей». Например, имя модуля `A.B` обозначает подмодуль с именем ` B` в пакете с именем `A`. Точно так же, как использование модулей избавляет авторов различных модулей от необходимости беспокоиться о именах глобальных переменных друг друга, использование точечных имен модулей избавляет авторов многомодульных пакетов, таких как NumPy или Pillow, от необходимости беспокоиться об именах модулей друг друга.

Suppose you want to design a collection of modules (a “package”) for the uniform handling of sound files and sound data. There are many different sound file formats (usually recognized by their extension, for example: `.wav`, `.aiff`, `.au`), so you may need to create and maintain a growing collection of modules for the conversion between the various file formats. There are also many different operations you might want to perform on sound data (such as mixing, adding echo, applying an equalizer function, creating an artificial stereo effect), so in addition you will be writing a never-ending stream of modules to perform these operations. Here’s a possible structure for your package (expressed in terms of a hierarchical filesystem):

Предположим, вы хотите создать набор модулей («пакет») для унифицированной обработки звуковых файлов и звуковых данных. Существует много различных форматов звуковых файлов (обычно распознаваемых по их расширению, например: `.wav`, ` .aiff`,  `.au`), поэтому может потребоваться создать и поддерживать растущую коллекцию модулей для преобразования между различными форматами файлов. Есть также много различных операций, которые вы можете выполнять над звуковыми данными (например, микширование, добавление эха, применение функции эквалайзера, создание искусственного стереоэффекта), поэтому, кроме того, вы будете писать бесконечный поток модулей для выполнения этих операций. Вот возможная структура вашего пакета (выраженная в терминах иерархической файловой системы):

```
sound/                         Пакет верхнего уровня
      __init__.py              Инициализация звукового пакета
      formats/                 Подпакет для преобразований форматов файлов
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                 Подпакет для звуковых эффектов
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                 Подпакет для фильтров
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

При импорте пакета Python просматривает каталоги в `sys.path` в поисках подкаталога пакета.

Файлы `__init __.py` необходимы, чтобы Python рассматривал каталоги, содержащие файл, как пакеты. Это не допускает каталоги с общим именем, такие как `string`, непреднамеренно скрывая допустимые модули, которые появляются позже в пути поиска модулей. В простейшем случае `__init __.py` может быть пустым файлом, но он также может выполнить код инициализации для пакета или установить переменную ` __all__`, как описано ниже.

Пользователи пакета могут импортировать отдельные модули из пакета, например:

```
import sound.effects.echo
```

Это загружает подмодуль `sound.effects.echo`. На него должно быть указано полное имя.

```
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```

Альтернативный способ импорта подмодуля:

```
from sound.effects import echo
```

Это также загружает подмодуль `echo` и делает его доступным без префикса пакета, поэтому его можно использовать следующим образом:

```
echo.echofilter(input, output, delay=0.7, atten=4)
```

Еще один вариант - импортировать нужную функцию или переменную напрямую:

```
from sound.effects.echo import echofilter
```

Again, this loads the submodule `echo`, but this makes its function `echofilter()` directly available:

Опять же, это загружает подмодуль `echo`, но это делает его функцию ` echofilter() ` доступной непосредственно:

```
echofilter(input, output, delay=0.7, atten=4)
```

Обратите внимание, что при использовании `from package import item`, элемент может быть либо подмодулем (или подпакетом) пакета, либо каким-либо другим именем, определенным в пакете, например функцией, классом или переменной. Оператор `import` сначала проверяет, определен ли элемент в пакете; если нет, он предполагает, что это модуль и пытается загрузить его. Если он не может его найти, возникает исключение [`ImportError`](https://docs.python.org/3/library/exceptions.html#ImportError).

Наоборот, при использовании синтаксиса, такого как `import item.subitem.subsubitem`, каждый элемент, кроме последнего, должен быть пакетом; последний элемент может быть модулем или пакетом, но не может быть классом, функцией или переменной, определенной в предыдущем элементе.

### 2.4.1. Импорт * из пакета

Что происходит, когда пользователь пишет `from sound.effects import *`? В идеале можно надеяться, что это каким-то образом позволит перейти в файловую систему, найти, какие подмодули присутствуют в пакете, и импортировать их все. Это может занять много времени, и импорт подмодулей может иметь нежелательные побочные эффекты, которые должны происходить, только если подмодуль явно импортирован.

The only solution is for the package author to provide an explicit index of the package. The [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) statement uses the following convention: if a package’s `__init__.py` code defines a list named `__all__`, it is taken to be the list of module names that should be imported when `from package import *` is encountered. It is up to the package author to keep this list up-to-date when a new version of the package is released. Package authors may also decide not to support it, if they don’t see a use for importing * from their package. For example, the file `sound/effects/__init__.py` could contain the following code:

Единственное решение для автора пакета - предоставить явный индекс пакета. Оператор [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) использует следующее соглашение: если код пакета `__init__.py` определяет список с именем ` __all__`, он считается списком имен модулей, которые должны быть импортированы при использовании `from package import *`. Автор пакета должен поддерживать этот список в актуальном состоянии, когда выходит новая версия пакета. Авторы пакета могут также принять решение не поддерживать его, если они не видят смысла в импорте * из своего пакета. Например, файл `sound/effects/__init__.py` может содержать следующий код:

```
__all__ = ["echo", "surround", "reverse"]
```

This would mean that `from sound.effects import *` would import the three named submodules of the `sound` package.

Это означает, что `from sound.effects import *` будет импортировать три подмодуля пакета `sound`.

If `__all__` is not defined, the statement `from sound.effects import *` does *not* import all submodules from the package `sound.effects` into the current namespace; it only ensures that the package `sound.effects` has been imported (possibly running any initialization code in `__init__.py`) and then imports whatever names are defined in the package. This includes any names defined (and submodules explicitly loaded) by `__init__.py`. It also includes any submodules of the package that were explicitly loaded by previous [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) statements. Consider this code:

Если `__all__` не определено, оператор ` from sound.effects import * `  *не* импортирует все подмодули из пакета ` sound.effects` в текущее пространство имен; он только гарантирует, что пакет `sound.effects` был импортирован, а затем импортирует все имена, определенные в пакете. Это включает любые имена, определенные (и явно загруженные подмодули) с помощью `__init__.py`. Он также включает любые подмодули пакета, которые были явно загружены предыдущими операторами [`import`](https://docs.python.org/3/reference/simple_stmts.html#import). Рассмотрим этот код:

```
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```

В этом примере модули `echo` и `round` импортируются в текущее пространство имен, потому что они определены в пакете` sound.effects` при выполнении оператора `from ... import`. (Это также работает, когда определено `__all__`.)

Хотя некоторые модули предназначены для экспорта только имен, которые следуют определенным шаблонам, когда вы используете `import *`, в производственном коде это все еще считается плохой практикой.

Помните, что нет ничего плохого в использовании `from package import specific_submodule`! Фактически, это рекомендуемая запись, если импортирующему модулю не нужно использовать подмодули с одинаковыми именами из разных пакетов.

### 2.4.2. Внутрипакетные ссылки

When packages are structured into subpackages (as with the `sound` package in the example), you can use absolute imports to refer to submodules of siblings packages. For example, if the module `sound.filters.vocoder` needs to use the `echo` module in the `sound.effects` package, it can use `from sound.effects import echo`.

Когда пакеты структурированы в подпакеты (как в примере с `sound`), можно использовать абсолютное импортирование для ссылки на пакеты-потомки. Например, если модуль `sound.filters.vocoder` должен использовать модуль ` echo` в пакете `sound.effects`, он может использовать ` from sound.effects import echo`.

Также можно использовать относительное импортирование, используя форму `from module import name`. В этих импортах используются символы точек для указания текущих и родительских пакетов, участвующих в относительном импорте. Например, для модуля `round` вы можете использовать:

```
from . import echo
from .. import formats
from ..filters import equalizer
```

Note that relative imports are based on the name of the current module. Since the name of the main module is always `"__main__"`, modules intended for use as the main module of a Python application must always use absolute imports.

Обратите внимание, что относительное импортирование основано на имени текущего модуля. Поскольку имя основного модуля всегда `"__main__"`, модули, предназначенные для использования в качестве основного модуля приложения Python, всегда должны использовать абсолютный импорт.

### 2.4.3. Пакеты в нескольких каталогах

Пакеты поддерживают еще один специальный атрибут, [`__path__`](https://docs.python.org/3/reference/import.html#__path__). Он инициализируется как список, содержащий имя каталога, в котором содержится `__init__.py` пакета перед выполнением кода в этом файле. Эта переменная может быть изменена; это влияет на последующий поиск модулей и подпакетов, содержащихся в пакете.

Хотя эта функция не требуется часто, ее можно использовать для расширения набора модулей, находящихся в пакете.

## Математика

Модуль [`math`](https://docs.python.org/3/library/math.html#module-math) предоставляет доступ к базовым функциям библиотеки C для математики с плавающей точкой:

\>>>

```
>>> import math
>>> math.cos(math.pi / 4)
0.70710678118654757
>>> math.log(1024, 2)
10.0
```

Модуль [`random`](https://docs.python.org/3/library/random.html#module-random) предоставляет инструменты для случайного выбора:

\>>>

```
>>> import random
>>> random.choice(['apple', 'pear', 'banana'])
'apple'
>>> random.sample(range(100), 10)   # sampling without replacement
[30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
>>> random.random()    # random float
0.17970987693706186
>>> random.randrange(6)    # random integer chosen from range(6)
4
```

The [`statistics`](https://docs.python.org/3/library/statistics.html#module-statistics) module calculates basic statistical properties (the mean, median, variance, etc.) of numeric data:

Модуль [`statistics`](https://docs.python.org/3/library/statistics.html#module-statistics) вычисляет основные статистические свойства (среднее значение, медиана, дисперсия и т. д.) числовых данных:

\>>>

```
>>> import statistics
>>> data = [2.75, 1.75, 1.25, 0.25, 0.5, 1.25, 3.5]
>>> statistics.mean(data)
1.6071428571428572
>>> statistics.median(data)
1.25
>>> statistics.variance(data)
1.3720238095238095
```

В библиотеке [SciPy](https://scipy.org/) есть множество других модулей для численных расчетов.