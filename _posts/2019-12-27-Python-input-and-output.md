---
layout: post
title: Python输入和输出
date: 2019-12-27 14:00:00 +0900
categories: [能工巧匠集, Python]
tags: [Output, python, sys, format]
---

## 7.输入和输出

有几种方式来展示程序的输出。数据可以以人们可读的形式打印出来或者写到一个文件中供后续使用。这一章节将会讨论一些可能性。（This chapter will discuss some of the possibilities）

## 7.1.更精彩的输出格式 (Fancier Output Formatting)

到目前为止我们已经遇到两种写值的方式：表达式语句和 [`print()`](https://docs.python.org/3.8/library/functions.html#print) 函数。第三种方式是使用文件对象的`write()`方法，标准的输出文件可以被引用为`sys.stdout`。有关的更多信息参见库参考）

经常，你希望控制你的输出格式而不仅仅是简单地打印出以空格隔开的值。有几种方式来格式化输出：

- 在一个字符串开头的引号或三引号外头加`f`或`F`来使用[格式化字符串文本 (formatted string literals)](https://docs.python.org/3.8/tutorial/inputoutput.html#tut-f-strings)。在这字符串中的`{`和`}`符号之间你可以写Python表达式来引用变量或文本值。

  ```python
  >>> year = 2016
  >>> event = 'Referendum'
  >>> f'Results of the {year} {event}'
  'Results of the 2016 Referendum'
  ```

- 字符串的 [`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 方法需要更多的人为努力，你将会使用`{`和`}`去标记在哪里用一个变量来替换以及提供详细的格式指导，但是你仍然需要提供格式化的信息。

  ```python
  >>> yes_votes = 42_572_654
  >>> no_votes = 43_132_495
  >>> percentage = yes_votes / (yes_votes + no_votes)
  >>> '{:-9} YES votes {:2.2%}'.format(yes_votes, precentage)
  '42572654 YES votes 49.67%'
  ```

- 最后，通过使用字符串切片和拼接操作你可以按自己想要的格式实现所有的字符串处理。字符串数据类型有一些方法可以执行有用的操作来在给定的字符宽度添加字符串。

在调试时，你不需要精彩的输出而仅仅想要快速显示一些变量的值，你可以用 [`repr()`](https://docs.python.org/3.8/library/functions.html#repr) 或 [`str()`](https://docs.python.org/3.8/library/stdtypes.html#str)函数将任意的值转为一个字符串。

[`str()`](https://docs.python.org/3.8/library/stdtypes.html#str) 函数返回人类可读的值的表现形式，而 [`repr()`](https://docs.python.org/3.8/library/functions.html#repr) 函数生成供解释器读取的值表现形式 (或者如果没有等价的语法，会强制抛出 [`SyntaxError`](https://docs.python.org/3.8/library/exceptions.html#SyntaxError) 异常)。对于没有特定表示供人类使用的对象，[`str()`](https://docs.python.org/3.8/library/stdtypes.html#str) 将返回跟 [`repr()`](https://docs.python.org/3.8/library/functions.html#repr)一样的值。很多值，比如像列表和字典这样的数值或结构，无论使用什么函数都返回相同的表现形式。特别地，`Strings`有两种明显不同的表现形式。

下面是一些例子：

```python
>>> s = 'Hello, world.'
>>> str(s)
'Hello, world.'
>>> repr(s)
"'Hello, world.'"
>>> str(1/7)
'0.14285714285714285'
>>> x = 10 * 3.25
>>> y = 200 * 200
>>> s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'
>>> print(s)
The value of x is 32.5, and y is 40000...
>>> # The repr() of a string adds string quotes and backslashes:
>>> hello = 'hello, world\n'
>>> hellos = repr(hello)
>>> print(hellos)
'hello, world\n'
>>> print(hello)
hello, world

>>> # The argument to repr() may be any Python object:
>>> repr((x, y, ('spam', 'eggs')))
"(32.5, 40000, ('spam', 'eggs'))"
```

[`string`](https://docs.python.org/3.8/library/string.html#module-string) 模块包含一个 [`Template`](https://docs.python.org/3.8/library/string.html#string.Template) 类还提供另一种方式在字符串中替换值，通过使用占位符`$x`并用字典中的值替换它们，但是提供的格式控制较少。

### 7.1.1.格式化字符串文本

[格式化字符串文本 (Formatted string literals)](https://docs.python.org/3.8/reference/lexical_analysis.html#f-strings) (又简称为`f-strings`) 可以让你在字符串里面包含Python表达式的值通过在字符串前加前缀`f`和`F`以及表达式写为`{表达式}`。

一个选择的格式化器可以跟随表达式，这允许更好地控制如何格式化值。下面的例子将pi圆整为3为小数：

```python
>>> import math
>>> print(f'The value of pi is approximately {math.pi:.3f}.')
The value of pi is approximately 3.142.
```

在`':'`之后添加一个整数，这填充一个最小的字符宽度，这对行对齐很有用：

```python
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
>>> for name, phone in table.items():
...     print(f'{name:10} ==> {phone: 10d}')
...
Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
```

其他的修改器可以被用来在值格式化前进行转换：`!a` 应用 [`ascii()`](https://docs.python.org/3.8/library/functions.html#ascii)，`!s` 应用 [`str()`](https://docs.python.org/3.8/library/stdtypes.html#str) 以及`!r` 应用 [`repr()`](https://docs.python.org/3.8/library/functions.html#repr)。

```python
>>> animals = 'eels'
>>> print(f'My hovercraft is full of {animals}.')
My hovercraft is full of eels.
>>> print(f'My hovercraft is full of {animals!r}.')
My hovercraft is full of 'eels'.
```

这些格式化设置的参考手册请参见 [Format Specification Mini-Language](https://docs.python.org/3.8/library/string.html#formatspec) 参考手册。

### 7.1.2.String 的 format() 方法

[`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 方法的基本使用方法如下这样：

```python
>>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))
We are the knights who say "Ni!"
```

括号及其里面的字符 (称为格式化域) 由 [`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 方法传进来的对象替代。括号中的数字可以用来引用由 [`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 方法传进来的对象的位置。

```python
>>> print('{0} and {1}'.format('spam', 'eggs'))
spam and eggs
>>> print('{1} and {0}'.format('spam', 'eggs'))
eggs and spam
```

如果在 [`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 方法中使用关键字参数，它们的值可以通过参数的名字来引用：

```python
>>> print('This {food} is {adjective}.'.format(food='spam', adjective='absolutely horrible'))
This spam is absolutely horrible.
```

位置和关键字参数可以任意组合：

```python
>>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred', other='Georg'))
The story of Bill, Manfred, and Georg.
```

如果你有一个很长的格式化字符串而你又不想将它们分开，如果你能通过名字而不是位置来引用要格式化的变量，这将会很好。通过传递一个字典和实用方括号`[]`来访问关键字将会很容易实现。

```python
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
>>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; Dcab: {0[Dcab]:d}'.format(table))
Jack: 4098; Sjoerd: 4127; Dcab: 8637678
```

这也可以通过使用`**`符号将`table`作为传递关键字参数传递来实现。

```python
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
>>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
Jack: 4098; Sjoerd: 4127; Dcab: 8637678
```

这与内建函数 [`vars()`](https://docs.python.org/3.8/library/functions.html#vars) 结合使用会特别有用，  [`vars()`](https://docs.python.org/3.8/library/functions.html#vars) 函数返回的是一个包含所有局部变量的字典。

例如，下面的几行代码产生一组整齐排列的列，这些列给出整数及其平方和立方：

```python
>>> for x in range(1, 11):
...     print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))
...
 1   1    1
 2   4    8
 3   9   27
 4  16   64
 5  25  125
 6  36  216
 7  49  343
 8  64  512
 9  81  729
10 100 1000
```

使用 [`str.format()`](https://docs.python.org/3.8/library/stdtypes.html#str.format) 进行字符串格式化的完整概述, 参见 [格式化字符串语法 (Format String Syntax)](https://docs.python.org/3.8/library/string.html#formatstrings).

### 7.1.3.手动字符串格式化

这是手动进行格式化的平方和立方数的同样的表：

```python
>>> for x in range(1, 11):
...     print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
...		# 注意在前一行使用了'end'
...     print(repr(x*x*x).rjust(4))
...
 1   1    1
 2   4    8
 3   9   27
 4  16   64
 5  25  125
 6  36  216
 7  49  343
 8  64  512
 9  81  729
10 100 1000
```

(注意到 [`print()`](https://docs.python.org/3.8/library/functions.html#print) 函数在每一列之间添加了一个空格：它经常在它的参数之间添加空格。)

字符串对象的 [`str.rjust()`](https://docs.python.org/3.8/library/stdtypes.html#str.rjust) 方法右对齐给定宽度字段中的字符串通过在其左侧填充空格。有类似的方法[`str.ljust()`](https://docs.python.org/3.8/library/stdtypes.html#str.ljust) 和 [`str.center()`](https://docs.python.org/3.8/library/stdtypes.html#str.center)。这些方法不写任何东西，它们只返回一个新字符串。如果输入字符串太长，它们不会截断它，而是原样返回；这将打乱您的列布局，但这通常比另一种选择 (对值进行截断，**译者注**) 更好，后者会对某个值撒谎。(如果你真的想要截断，你可以添加一个切片操作，就像在 `x.ljust(n)[:n]` 中那样。)

还有另一个方法，[`str.zfill()`](https://docs.python.org/3.8/library/stdtypes.html#str.zfill)，它用0填充数字字符串的左侧。它能理解加号和减号:

```python
>>> '12'.zfill(5)
'00012'
>>> '-3.14'.zfill(7)
'-003.14'
>>> '3.14159265359'.zfill(5)
'3.14159265359'
```

### 7.1.4.旧的字符串格式化

`%`操作符也可以用来字符串的格式化，它很像一个`sprintf()`风格格式化字符串一样解析左边的参数用于右边的参数，返回一个格式化的字符串，比如：

```python
>>> import math
>>> print('The value of pi is approximately %5.3f.' % math.pi)
The value of pi is approximately 3.142.
```

更多的信息参见 [printf-风格字符串格式化 (printf-style String Formatting)](https://docs.python.org/3.8/library/stdtypes.html#old-string-formatting) 章节。

## 7.2.读写文件

[`open()`](https://docs.python.org/3.8/library/functions.html#open) 返回一个[文件对象 (file object)](https://docs.python.org/3.8/glossary.html#term-file-object)，并且大多数情况下需要两个参数：`open(filename, mode)`。

```python
>>> f = open('workfile', 'w')
```

第一个参数是一个包含字符串的文件名，第二个参数是包含几个字母来描述文件被使用的方式。`mode`可以是`r`当文件只读，`‘w’`表示文件可写 (存在同名的文件将会被删除)，`‘a’`表示以追加的方式打开文件，即任意的数据都被自动添加到文件末尾。`’r+’`表示以可读可写的方式来打开文件。`mode`参数是可选的，默认是`‘r’`。

通常，文件是以文本文件的格式打开，这意味着你可以像文件读和写字符串，它们以特定的字符编码进行编码。如果没有指定编码格式，那么编码形式由平台决定 (参见 [`open()`](https://docs.python.org/3.8/library/functions.html#open))。`'b'`添加到`mode`中表示以二进制模式打开文件：这个时候数据以字节对象的形式进行读和写，这种模式应该用于所有不包含文本的文件。

在文本文件模式中，在读取文件时默认会将不同平台下的行结束符 (Unix系统为`\n`，Windows系统为`\r\n`) 转换为`\n`，在写文件时默认会将`\n`转换为不同平台下的行结束符。这些隐藏的文件数据修改对文本文件没问题，但是对像`JPEG`或`EXE`这样的二进制文件则会崩溃，因此，在读写这样的文件时请谨慎使用二进制模式。

在处理文件对象时使用 [`with`](https://docs.python.org/3.8/reference/compound_stmts.html#with) 关键字是一个很好的习惯，它的好处是在处理文件结束是会在合适的时候关闭文件，尽管在某些情况会抛出异常。同时，使用`with`比写等价的 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try)-[`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally)代码块更短：

```python
>>> with open('workfile') as f:
...		read_data = f.read()
	
>>> # 我们可以检查文件已经被自动关闭
>>> f.closed
True
```

如果你没有使用 [`with`](https://docs.python.org/3.8/reference/compound_stmts.html#with) 关键字，那么你应该调用`f.close()`来关闭文件并立即释放它所使用的系统资源。如果你没有显示关闭一个文件，Python垃圾收集器最终会销毁并帮你关闭文件，但是期间文件一直处于打开状态，而且存在另一个风险，不同的Python实现会在不同的时间来清楚这个文件对象。

无论是使用 [`with`](https://docs.python.org/3.8/reference/compound_stmts.html#with) 语句还是调用`f.close()`，在文件关闭后，试图使用文件会自动失败：

```python
>>> f.close()
>>> f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file. 
```

### 7.2.1.文件对象方法

本章节剩下的例子是假设已经创建一个文件对象`f`。

要读取一个文件的内容，请调用`f.read(size)`，这个函数从文件中读取一定量的数据并返回一个字符串 （文本文件模式) 或字节对象 (二进制模式)，其中`size`是一个可选参数。当省略`size`或为负值，将读取整个文件的内容并返回，如果文件的大小是你电脑内存的两倍，那么则存在问题。否则，最多读取并返回`size`个字符 (文本模式) 或字节 (二进制模式)。如果已经到达文件尾，`f.read()`将返回一个空字符 (`''`)。

```python
>>> f.read()
'This is the entire file.\n'
>>> f.read()
''
```

`f.readline()`从文件中读取一行，并且在字符串尾部保持换行符 (`\n`)，如果文件不是以空行结束 (在Windows中，空行结束意味着你要在文件尾按**两次**回车，而在Linux中只需按一次回车)，那么在文件尾时其返回依然是一个空字符 (`''`) 而不会包含换行符 (`\n`)，这使得它的返回值是不清楚的。如果`f.readline()`返回一个空字符串，那么说明已经到达文件尾，如果空行以`\n` 表示，返回的字符串仅包含一个新行。

```python
>>> f.readline()
'This is the first line of the file.\n'
>>> f.readline()
'Second line of the file\n'
>>> f.readline()
''
```

要读取文件中多行，你可以循环遍历文件对象，这种做法快速、高效以及代码简单：

```python
>>> for line in f:
...		print(line, end='')
...
This is the first line of the file.
Second line of the file
```

如果你想读取文件中所有的行并放在一个列表中，你也可以用`lis(f)`或`f.readlines()`。

`f.write(string)`将字符串`string`的内容写到文件中并返回写入的字符大小。

```python
>>> f.write('This is a test\n')
15
```

其他类型的数据在写进文件之前需要转换为字符串 (文本模式) 或者字节对象 (二进制模式):

```python
>>> value = ('the answer', 42)
>>> s = str(value)		# 将元组转化为字符串
>>> f.write(s)
18
```

`f.tell()`返回一个整数表明文件对象在文件中当前所处的位置，二进制模式下，返回的是由文件头开始算起的字节数；在文本文件模式下，返回的是一个不透明的数 (**opaque number???**)。

要改变文件对象所处的位置，可以使用`f.seek(offset, whence)`函数。新的位置是由给定的参考点加上偏移量 (`offset`) 来计算。其中，参考点有`whence`参数选定，其值为**0**时，表示**文件头**；为**1**时，表示**当前位置**；为**2**时，表示**文件尾**。`whence`被省略时，采用**默认值0**，即文件头作为参考点。

```python
>>> f = open('workfile', 'rb+')		# 注意，这个workfile文件需要提前建好
>>> f.write(b'0123456789abcdef')
16
>>> f.seek(5)		# 去文件的第6个字节
5
>>> f.read(1)
b'5'
>>> f.seek(-3, 2)		# 去文件的倒数第3个字节
13
>>> f.read(1)
b'd'
```

在文本文件中 (那些在模式化字符串中没有带`b`的字符串)，只允许相对文件头来搜索定位 (其中一个例外是使用`seek(0, 2)` 刚好定位到文件尾 the exception being seeking to the very file end with `seek(0,2)`)，而且偏移值`offset`只对由`f.tell()`返回的值或0有效，使用其他的偏移值会产生意想不到的行为。

文件还有其他的方法，比如用得比较少的`isatty()`和`truncate()`，文件对象的完整使用手册可以参考库参考。

###  7.2.2.使用 [`json`](https://docs.python.org/3.8/library/json.html#module-json) 保存结构化数据

字符串很写到文件和从文件中读取，而数字需要更多的努力，因为`read()`方法仅返回字符串，需要传递给函数如 [`int()`](https://docs.python.org/3.8/library/functions.html#int)进行转换，它接收一个字符串如`'123'`则返回对应的数值`123`。当你想要保存更复杂的数据类型，比如嵌套列表和字典，通过手动解析和序列化则会变得很复杂。

Python允许用户使用流行的数据交换格式 [JSON (JavaScript对象表示法)](http://json.org/)，而不是让用户不断地编写和调试代码来将复杂的数据类型保存到文件中。名为 [`json`](https://docs.python.org/3.8/library/json.html#module-json) 的标准模块可以将Python数据层次结构化，并将其转化为字符串表示，这个过程称为序列化。反之，将字符串形式重新构造数据称为反序列化。在序列化和反序列化之间，用字符串表示的对象可以存储在一个文件或数据中，或者通过网络输送到一些远程机器上。

**注意**

> 在现代应用程序中，将JSON格式用于数据交换已经很常见的了，很多程序员对它也很熟悉，这使得它成为用于互通性的很好选择。

如果你有一个对象`x`，你可以通过简单的代码就可以查看它的JSON字符串表示:

```python
>>> import json
>>> json.dumps([1, 'simple', 'list'])
'[1, "simple", "list"]'
```

[`dumps()`](https://docs.python.org/3.8/library/json.html#json.dumps) 函数的另一个变体称为 [`dump()`](https://docs.python.org/3.8/library/json.html#json.dump)，它可以简单地将对象序列化为[文本文件 (text file)](https://docs.python.org/3.8/glossary.html#term-text-file)，因此，如果`f`是打开用来写数据的 [文本文件 (text file)](https://docs.python.org/3.8/glossary.html#term-text-file) 对象，我们可以这样做：

```python
json.dump(x, f)
```

如果`f`是一个打开用来读取数据的 [文本文件 (text file)](https://docs.python.org/3.8/glossary.html#term-text-file) 对象，通过如下代码将对象进行解码：

```python
x = json.load(f)
```

这种简单的序列化技术可以处理列表和字典，但是要想将任意的类实例转换成JSON就需要更多的努力了， [`json`](https://docs.python.org/3.8/library/json.html#module-json) 模块的参考文档包含这方面的解释。

> 也可以参见 [`pickle`](https://docs.python.org/3.8/library/pickle.html#module-pickle)  - pickle模块
>
> 与 [JSON](https://docs.python.org/3.8/tutorial/inputoutput.html#tut-json) 相反，pickle是一种可以用来序列化任意复杂Python对象的协议。同样地，它是专门用于Python的，无法与用其他编程语言写的应用程序进行通讯。默认情况下它也不安全：如果数据是由老练的攻击者精心设计的，那么不可信来源的反序列化pickle数据可以执行任意代码。
