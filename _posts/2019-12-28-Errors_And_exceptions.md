---
layout: post
title: Python错误和异常
date: 2019-12-28 01:00:00 +0900
categories: [能工巧匠集, Python]
tags: [errors, python, function, exceptions]
---

[<<7.输入与输出](../Python-input-and-output)--[9.类和对象>>](../Python-class-and-object)

# 8.错误和异常

直到现在，还没提到过错误信息，但是如果你已经尝试一些例子，你会看到一些。有两种 (至少) 明显类型的错误：语法错误和异常。

## 8.1.语法错误

语法错误，也称为解析错误，可能是你在学习Python时遇到的最常见的抱怨：

```python
>>> while True print('Hello world')
  File "<stdin>", line 1
  	while True print('Hello world')
  				  ^
SyntaxError: invalid syntax  
```

解析器重复出错的行，并显示一个小箭头，指向该行中检测到距离错误最近点。这个错误是由箭头之前的标记 (或至少在该标记处检测到) 引起的: 在本例中，错误是在 [`print()`](https://docs.python.org/3.8/library/functions.html#print)函数处检测到的，因为在它之前缺少一个冒号(`':'`)。文件名和行号将被打印出来，这样您就知道在输入来自脚本的情况下应该查找哪里。

## 8.2.异常

即使语句或表达式在语法上是正确的，在试图执行它时也可能导致错误。在**执行过程**中检测到的错误称为**异常**，并且不是无条件致命 (unconditionally fatal) 的: 您将很快了解如何在Python程序中处理它们。但是，大多数异常都不是由程序处理的，并且会导致如下所示的错误消息:

```python
>>> 10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
>>> 4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Can't convert 'int' object to str implicitly  
```

错误消息的最后一行指示发生了什么。异常有不同的类型，并且类型作为消息的一部分打印: 示例中的类型有[`ZeroDivisionError`](https://docs.python.org/3.8/library/exceptions.html#ZeroDivisionError)、[`NameError`](https://docs.python.org/3.8/library/exceptions.html#NameError) 和 [`TypeError`](https://docs.python.org/3.8/library/exceptions.html#TypeError)。打印为异常类型的字符串是发生的内置异常的名称，对于所有的内置异常都是这样，但是对于用户定义的异常不一定是这样 (尽管这是一个有用的约定)。标准异常名是内置的标识符 (而不是保留关键字)。这一行的其余部分提供了基于异常类型及其原因的详细信息。

错误消息的前一部分以堆栈回溯的形式显示了异常发生的上下文。通常它包含一个堆栈回溯清单源代码行，但是，它不会显示从标准输入读取的行。

[内置异常 (Built-in Exceptions](https://docs.python.org/3.8/library/exceptions.html#bltin-exceptions) 列出了内置异常及其含义。

## 8.3.异常处理

可以对选定的异常编写程序来处理。请看下面的例子，它要求用户输入直到输入了一个有效的整数，但是允许用户中断程序 (使用Ctrl-C或操作系统支持的任何方法)；请注意，用户生成的中断是通过引发 [`KeyboardInterrupt`](https://docs.python.org/3.8/library/exceptions.html#KeyboardInterrupt) 异常来发出信号的。

```python
>>> while True:
...		try:
...			x = int(input("Please enter a number: "))
...			break
...		except valueError:
...			print("Oops! That was no valid number. Try again...")
...			
```

[`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句的工作流程如下：

- 第一，*try分支* ([`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 和 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 关键词之间的语句) 被执行。
- 如果没有异常发生，*except分支*被跳过以及 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句的执行完成。
- 如果在执行try分支过程中发生异常，那么该分支的剩余部分则被跳过。接着，如果异常的类型与 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 关键字后命名的异常匹配，则执行except分支，然后继续执行 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句 (and then execution continues after the try statement)。
- 如果所发生的异常与 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 关键字后命名的异常不匹配，那么就跳出 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句。如果没有找到处理程序，它是一个*未处理的异常*，执行将停止，并显示一条如上所示的消息。

[`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句可以有多个 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支，用于为不同的异常指定处理程序，而最多执行其中的一个处理程序。处理程序只处理发生在相应 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句中的异常，而不会处理同一 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句中不同分支的异常。一个 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支可以将多个异常用括号括起来命名为元组，比如：

```python
... except (RuntimrError, TypeError, NameError):
... 	pass
```

如果 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支中列出的类是同一个类或基类，那么该类与抛出的异常类兼容 (如果 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支列出的派生类与基类不兼容，则与抛出的异常类不兼容)。比如，下面的代码依次打印B、C、D:

```python
class B(Exception):
	pass
	
class C(B):
	pass

class D(C):
	pass

for cls in [B, C, D]:
	try:
		raise cls()
	except D:
		print("D")
	except C:
		print("C")
	except B:
		print("B")
```

注意，如果except分支的顺序相反 (`except B` 在第一)，它将会打印出: B、B、B — 第一个匹配的except分支会被触发。

**最后一个 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支**可以省略异常名，以作为通配符。使用这种方式要特别小心，因为它很容易掩盖程序的真实错误！它也可以打印一个错误信息并重新抛出异常 (也允许一个调用者来处理异常)。

```python
import sys

tri:
	f = open('myfile.txt')
	s = f.readline()
	i = int(s.strip())
except OSError as err:
	print("OS errorL {0}".format(err))
except ValueError:
	print("Could not convert data to an integer.")
except:
	print("Unexpected error:", sys.exc_info()[0])
	raise
```

[`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) ... [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 语句存在一个可选的else分支，当它出现的时候，必须是跟在所有的except分支之后。它对try分支不会抛出异常而一些代码又必须执行时非常有用。比如：

```python
for arg in sys.argv[1:]:
	try:
		f = open(arg, 'r')
	except OSError:
		print('cannot open', arg)
	else:
		print(arg, 'has', len(f.readlines()), 'lines')
		f.close()
```

使用`else`分支要比在 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 分支中添加额外的代码要好，因为它可以避免意外捕获由`try` ... `except`语句保护的代码没有引发的异常。

当一个异常发生时，它会有一个相关的值，也就是异常的参数，参数的出现和类型取决于异常类型。

异常分支可以在异常名之后定义一个变量，这个变量通过存储在`instance.args`中的参数绑定在一个异常实例。为了方便，异常实例定义了 [`__str__()`](https://docs.python.org/3.8/reference/datamodel.html#object.__str__) 来直接打印参数而无需引用`.args`。在异常抛出之前也可以实例化一个异常，并向它添加任意想要的属性。

```python
>>> try:
...		raise Exception('spam', 'eggs')
...	except Exception as inst:
...		print(type(inst))		# 异常的实例
...		print(inst.args)		# 存储在 .args 中的参数
... 	print(inst)				# __str__ 允许直接打印 args
...								# 但是在异常子类中可能会被覆盖
...		x, y = inst.args		# 对 args 进行解包
...		print('x = ', x)
...		print('y = ', y)
...
<class 'Exception'>
('spam', 'eggs')
('spam', 'eggs')
x = spam
y = eggs
```

如果异常有参数，它们作为为处理异常的最后部分 (详细信息) 被打印出来。

异常处理程序不仅仅处理直接发生在 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 分支的异常，还会处理在 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 分支中调用的函数 (甚至是间接调用) 内部发生的异常。比如：

```python
>>> def this_fails():
... 	x = 1/0
...
>>> try:
...		this_fails()
...	except ZeroDivisionError as err:
...		print('Handling run-time error:', err)
...
Handling run-time error: division by zero
```

## 8.4.抛出异常

 [`raise`](https://docs.python.org/3.8/reference/simple_stmts.html#raise) 语句允许程序员强制引发 (或抛出) 特定的异常。比如：

```python
>>> raise NameError('HiThere')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: HiThere
```

 [`raise`](https://docs.python.org/3.8/reference/simple_stmts.html#raise) 唯一的参数是所要抛出的异常，这必须是一个异常实例或一个异常类 (由 [`Exception`](https://docs.python.org/3.8/library/exceptions.html#Exception) 派生的类)。如果一个异常类被传递，它隐式调用其不带参数的构造函数进行实例化。

```
raise ValueError		# 'raise ValueError()' 的简短写法
```

如果你需要决定一个异常是否被抛出但不想处理它，一种更简单的 [`raise`](https://docs.python.org/3.8/reference/simple_stmts.html#raise) 语句允许你重新抛出这个异常：

```python
>>> try:
...		raise NameError('HiThere')
...	except NameError:
...		print('An exception flew by!')
...		raise
...
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: HiThere
```

## 8.5.用户自定义异常

程序可以通过创建一个异常类 (参见[类 (Classes)](https://docs.python.org/3.8/tutorial/classes.html#tut-classes) 来了解更多的Python类) 来命名自己的异常。异常通常直接或间接地派生于 [`Exception`](https://docs.python.org/3.8/library/exceptions.html#Exception) 类。

异常类可以被定义成其他任意类可做的事情，但是通常保持简单，仅提供几个属性供处理程序提取异常的错误信息。当定义一个模块可以抛出多个不同的错误时，一个常用的实践是为这个模块定义的异常创建一个基类，然后对应不同的错误条件来创建特定的子类：

```python
class Error(Exception):
	"""这个模块异常的基类"""
	pass

class InputError(Error):
	"""输入错误所抛出的异常
	
	Attributes:
		expression -- 错误发生时的表达式input expression in which the error occurred
		message -- 对错误的解释expanation of the error
	"""
	
	def __init__(self, expression, message):
		self.expression = expression
		self.message = message
	
class TransitionError(Error):
	"""操作符试图进行不允许的状态转换引起的异常
	
	Attributes:
		previous -- 转换前的状态
		next -- 试图转换的新状态
		message -- 解释为什么特定的转换不允许
	"""
	
	def __init__(self, previous, next, message):
		self.previous = previous
		self.next = next
		self.message = message
```

大多数的异常使用以"Error"结尾的名字命名，与标准异常命名类似。

很多标准模块都定义了自己的异常来对其中定义的函数可能引起的错误进行报告。与类有关的更多详细信息在[类 (Classes)](https://docs.python.org/3.8/tutorial/classes.html#tut-classes)这一章进行描述。

## 8.6.定义清除动作 (Defining Clean-up Actions)

[`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句还有另一个可选分支，用来定义在所有情况下都必须执行的清除动作，比如：

```python
>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
...
Goodbye, world!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
KeyboardInterrupt
```

如果存在 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支，它会作为 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句完成前的最后任务来执行。无论 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句是否产生异常，[`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支都会执行。下面的几点讨论当异常发生时更复杂的情况：

- 如果异常在 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 分支执行过程中发生，那么它会被一个 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支处理；如果异常没有被其中一个 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 分支处理，那么它会在 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支执行完成后重新被抛出。

- 在执行`except`或`else`分支过程中发生异常，同样，异常会在 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支执行完成后重新被抛出。

- 如果 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 语句执行到 [`break`](https://docs.python.org/3.8/reference/simple_stmts.html#break)、 [`continue`](https://docs.python.org/3.8/reference/simple_stmts.html#continue) 或 [`return`](https://docs.python.org/3.8/reference/simple_stmts.html#return)语句，那么 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支会先于 [`break`](https://docs.python.org/3.8/reference/simple_stmts.html#break)、 [`continue`](https://docs.python.org/3.8/reference/simple_stmts.html#continue) 或 [`return`](https://docs.python.org/3.8/reference/simple_stmts.html#return) 语句执行。

- 如果 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支包含一个 [`return`](https://docs.python.org/3.8/reference/simple_stmts.html#return)  语句，[`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支的 [`return`](https://docs.python.org/3.8/reference/simple_stmts.html#return)  语句会先于 [`try`](https://docs.python.org/3.8/reference/compound_stmts.html#try) 分支中的 [`return`](https://docs.python.org/3.8/reference/simple_stmts.html#return) 语句执行。

比如：

```python
>>> def bool_return():
...		try:
...			return True
...		finally:
...			return False
...
>>> bool_return()
False
```

一个更复杂的实例：

```python
>>> def divide(x, y):
...		try:
...			result = x / y
...		except ZeroDivisionError:
...			print("division by zero!")
...		else:
...			print("results is", result)
...		finally:
...			print("executing finally clause")
...
>>> divide(2, 1)
result is 2.0
executing finally clause
>>> divide(2, 0)
division by zero!
executing finally clause
>>> divide("2", "1")
executing finally clause
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in divide
TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

正如你所见，[`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支在任何的事件下都会被执行。由两个字符串相除引起的 [`TypeError`](https://docs.python.org/3.8/library/exceptions.html#TypeError) 没有被 [`except`](https://docs.python.org/3.8/reference/compound_stmts.html#except) 语句处理，因此在 [`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 执行后重新被抛出。

在实际的应用程序中，无论资源的使用成功与否，[`finally`](https://docs.python.org/3.8/reference/compound_stmts.html#finally) 分支对于释放外部资源 (如文件或网络连接) 都很有用。

## 8.7.预定义的清理动作 (Predefined Clean-up Actions)

有些对象定义了再不再需要对象时执行的标准清理操作，无论使用该对象的操作是否成功。请看下面的例子，它尝试打开一个文件并将文件的内容打印到屏幕上：

```python
for line in open("myfile.txt"):
	print(line, end="")
```

这段代码存在的问题是，在代码执行完成后，文件依然会被打开一段不确定的时间，这对于简单的脚本不是什么问题，但是对于更大型的应用可能存在问题。[`with`](https://docs.python.org/3.8/reference/compound_stmts.html#with) 语句允许像文件这样的对象以一种确保它们总是被及时正确地清理干净的方式所使用。

```python
with open("myfile.txt") as f:
	for line in f:
		print(line, end="")
```

在语句执行之后，文件`f`通常会被关闭，即使在处理这些行的过程中会遇到问题。与文件一样，提供预定义清理动作的对象会在它们的文档中说明这一点。
