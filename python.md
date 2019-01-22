# Google python Coding Style Guide - Google python编程规范指南
译者：[Emily-Tang](https://github.com/Emilylulu)<br>
转载请注明出处。<br>
欢迎PR。 - Pull Request is welcomed!<br>
# 目录
1. [背景](#1-背景)<br>
2. [python语言规则](#2-python语言规则)<br>
2.1 [Lint](#21-Lint)<br>
2.2 [导入模块](#22-导入模块)<br>
2.3 [包](#23-包)<br>
2.4 [异常处理](#24-异常处理)<br>
2.5 [全局变量](#25-全局变量)<br>
2.6 [内部类和函数](#26-内部或嵌套或本地的类和函数)<br>
2.7 [解析式和生成器表达式](#27-解析式和生成器表达式)<br>
2.8 [默认迭代器和运算符](#28-默认迭代器和运算符)<br>
2.9 [生成器](#29-生成器)<br>
2.10 [Lambda 函数](#210-Lambda 函数)<br>
2.11 [条件表达式](#211-条件表达式)<br>
3.16 [命名](#316-命名)<br>
## 1 背景
python 是谷歌主要使用的动态语言。这篇指南主要讲述在python中应该做和不应该做的事项。
## 2 python语言规则
### 2.1 Lint
调用 ``pylint``，来调试代码
#### 2.1.1 定义
``pylint``是个可以用来对python代码进行debug和完善代码风格的工具。就像对于c和c++而言，编译器可以找到一些bug，``pylint``也可以为python找到这类bug。不过由于python动态语言的特性，一些警告或许是不对的，但这种错误的警告应该是十分罕见的。
#### 2.1.2 优点
可以捕捉到简单的错误，比如拼写错误，或者未给变量赋值就使用。
#### 2.1.3 缺点
``pylint``并不完美。为了要很好的使用，有时我们需要忽略给出的警告，完善``pylint``。
#### 2.1.4 使用
要确保``pylint``在你的代码上运行。
忽视一些不准确的警告，这样其他的问题不会被隐藏。忽视警告，你可以用下面这个命令
```
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
```
在``pylint``中，警告都会用象征性的名字命名。如果用象征性的名字不够明确，则可以再加上注释。
用这种方法可以很容易找到被忽视的警告。
你可以得到一系列``pylint``警告，通过：
```
pylint --list-msgs
```
针对某一条获得更多信息，通过：
```
pylint --help-msg=C6409
```
可以用``pylint: disable`` 替换掉旧的格式``pylint: disable-msg``
对于没使用过的参数的警告，可以在函数开始删掉对应的变量来忽视这条警告。永远记得要加上一条注释表明为什么要删掉。用``unused``去注释就是很好的选择。例如：
``` python3
def viking_cafe_order(spam, beans, eggs=None)：

    del beans, eggs  # Unused by vikings.
    
    return spam + spam + spam
```
另一种方法是用``_``当做识别没使用过的参数的符号，用``unused_``或者``_``作为这个参数的前缀。这些方式是允许的，但不鼓励这么做。前面两个通过变量名来传递参数，但第三个就不能确保参数没使用
### 2.2 导入模块
运用``import``语句导入包或模块，不能用来导入单独的类或函数。注意对于[类型注解](#31912-类型注解的导入)类型的库有特殊的导入方式。
#### 2.2.1 定义
使代码可以重复使用从一个模块到另一个模块。
#### 2.2.2 优点
命名使用简易，源代码识别简单。``x.obj``说明obj这个对象是在x这个模块里定义的。
#### 2.2.3 缺点
模块的名称仍旧有一些有重叠。有些模块的名称很长。
#### 2.2.4 使用
用``import x``来导入模块或是包
用``from x import y``当x是一个包的前缀而y是一个没有前缀的模块。
用``from x import y as z``当有两个名为y的模块被引用或是y的名字特别长时。
用``import y as z``当z是一个标准的缩写。例如：``np``来替代``numpy``。
例子如下，当要导入``sound.effects.echo``这个模块时
```python3
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```
不要用相对名称去导入。即使模块来自同一个包，也要用包的全名。这样可以避免同一个包导入两次。
类型注解的导入则是例外。
### 2.3 包
用模块的完整路径名来导入每个模块
#### 2.3.1 优点
避免模块的名称有重合。减少由于模块搜索路径不对而产生的导入错误。使查找模块更简单。
#### 2.3.2 缺点
很难配置代码，因为需要复制包的层层等级。但这并不构成问题如果用现代的配置平台。
#### 2.3.3 使用
在新的代码中用包的全名来导入模块。
导入应该像下面的例子中一样。<br>
正确的：
```python3
# 在代码中用完整的名字调用absl.flags（详细的）
import absl.flags
from doctor.who import jodie
FLAGS = absl.flags.FLAGS
```
```python3
# 在代码中只用模块名称来导入flags (常用方法).
from absl import flags
from doctor.who import jodie
FLAGS = flags.FLAGS
```
不正确的：(假设此文件和``jodie.py``都存在与``doctor/who/``)
```python3
# 不清楚想要的模块是什么以及将导入什么。  
# 实际的导入将取决于外在的sys.path.
# 哪个可能的jodie模块才是真正想要被导入的？
import jodie
```
尽管在某些环境下会发生，但这个目录应该不能假定存在于``sys.path``中，在这种情况中，``import jodie``则意味着代码将导入一个第三方或者顶层的一个名叫jodie的包，而不是本地文件``jodie.py``。
### 2.4 异常处理
异常是允许的，但使用必须小心。
#### 2.4.1 定义
异常是在某些特定条件下，或者为了解决某些错误，可以改变代码的正常的控制流。
#### 2.4.2 优点
代码的正常控制流不会被处理错误而干扰。在某些情况下，控制流可以跳过多个数据框，例如，一步返回N层嵌套函数，而不是执行包含错误的所有代码。
#### 2.4.3 缺点
可能会导致控制流变复杂。当调用库函数的时候，容易遗漏错误。
#### 2.4.4 使用
例外必须遵循以下条件：<br>
1）以``raise MyError('Error message')``或``raise MyError()``的形式来使用异常处理。不要使用两个参数的形式，例如``raise MyError, 'Error message'``。<br>
2）确保使用异常处理的类是有意义的。比如，使用``ValueError``来提醒如果传一个负数而想要得到的是正数的情况。不要使用``assert``语句。``assert``是用来检验公有API参数的。它是用来确认内部是正确的，而不是用来强制改变正确的程度或是表明一些不期待发生的事件已经发生。如果是后者情况，就应用``raise``语句来处理。<br>
正确的：
```python3
  def connect_to_next_port(self, minimum):
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.
    Raises:
      ValueError: If the minimum port specified is less than 1024.
      ConnectionError: If no available port is found.
    Returns:
      The new minimum port.
    """
    if minimum < 1024:
      raise ValueError('Minimum port must be at least 1024, not %d.' % (minimum,))
    port = self._find_next_open_port(minimum)
    if not port:
      raise ConnectionError('Could not connect to service on %d or higher.' % (minimum,))
    assert port >= minimum, 'Unexpected port %d when minimum was %d.' % (port, minimum)
    return port
```
错误的：
```python3
  def connect_to_next_port(self, minimum):
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.
    Returns:
      The new minimum port.
    """
    assert minimum >= 1024, 'Minimum port must be at least 1024.'
    port = self._find_next_open_port(minimum)
    assert port is not None
    return port
```
3）库或者包可能会定义自己的异常处理。这种情况下必须继承已有的异常处理类。异常处理的名字应该已``error``结尾，且不能用重复的名字。（``foo.FooError``）<br>   
4）不要catch所有的``except``语句，``Exception``或``StandardError``，除非正在做异常处理，或已经在最外层线程中（并且打印错误信息）
python可以catch所有的异常，包括名称拼写错误，sys.exit()，Ctrl+C，和其他任何不想被catch的所有类型的异常。<br>
5）尽量减少``try/catch``内部的代码数量。try越长，那就越有可能会把不希望当成是异常的当成异常处理。在这些情况下，``try/catch``中可能包含真正的代码错误。<br>
6）不管异常有没有在``try``当中出现，用``finally``来执行代码。这通常对清理有用，比如关闭文件。<br>
7）当捕捉到一个异常，用``as``来替代逗号。比如：<br>
```python3
try:
  raise Error()
except Error as error:
  pass
```
### 2.5 全局变量
避免使用全局变量
#### 2.5.1 定义
变量的声明是模块级别或者是类的属性
#### 2.5.2 优点
有些情况下很有用
#### 2.5.3 缺点
有可能会在导入模块的过程中改变模块的性能。因为在导入模块的一开始就会完成给全局变量的赋值。
#### 2.5.4 使用
避免使用全局变量。当使用变量时，模块级别的常量是允许也是鼓励的。比如：``MAX_HOLY_HANDGRENADE_COUNT = 3``。常量必须都要大写且用下划线分割。可以参照[命名](#316-命名)。
如果需要使用全局变量，变量的声明要在模块的级别，在模块内部命名时加上``_``。外部使用必须通过公有的模块级别的函数。可以参照[命名](#316-命名)。
### 2.6 内部或嵌套或本地的类和函数
嵌套本地的类和函数是允许的，只要用于关闭局部变量。内部类是可以的。
#### 2.6.1 定义
类可以在函数或者方法或者类内部定义。函数可以在函数或者方法内部定义。嵌套函数对封闭作用域中的变量拥有只读访问权限。
#### 2.6.2 优点
可以在非常局限的作用域当中定义类和函数。广泛用于装饰器。[ADT-抽象数据类型](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E8%B3%87%E6%96%99%E5%9E%8B%E5%88%A5)
#### 2.6.3 缺点
 本地或者嵌套类中的实例不可以被序列化（pickle）。嵌套的类和函数不能直接测试。嵌套会让外部函数名字变长，可读性变差。
#### 2.6.4 使用
使用过程中有一些注意事项。避免使用嵌套除非是有本地值的闭包。不要因为对模块的使用者隐藏而使用嵌套。在模块级别中，函数名加``_``这样仍旧可以直接被测试访问。
### 2.7 解析式和生成器表达式
可以在简单的情况下使用。
#### 2.7.1 定义
列表，字典和集合（list, dict, set）的解析式和生成器表达式提供了简介和高效的方式，一种不需要使用传统循环，``map()``, ``filter()``或者是``lambda``，就可以创建一个容器类型和迭代器。
#### 2.7.2 优点
简单的解析式创建起来要比列表，字典和集合更简单更清晰。生成器表达式创建更高效，因为不需要创建列表。
#### 2.7.3 缺点
复杂的解析式和生成器表达式可能很难理解。
#### 2.7.4 使用
简单情况下可以使用。每一个部分必须一行以内：mapping 表达式，``for``语句，filter 表达式。多个``for``语句或者filter 表达式是不允许的。当需要更复杂的时候用循环代替。<br>
正确的：
```python3
  result = [mapping_expr for value in iterable if filter_expr]

  result = [{'key': value} for value in iterable
            if a_long_filter_expression(value)]

  result = [complicated_transform(x)
            for x in iterable if predicate(x)]

  descriptive_name = [
      transform({'key': key, 'value': value}, color='black')
      for key, value in generate_iterable(some_input)
      if complicated_condition_is_met(key, value)
  ]

  result = []
  for x in range(10):
      for y in range(5):
          if x * y > 10:
              result.append((x, y))

  return {x: complicated_transform(x)
          for x in long_generator_function(parameter)
          if x is not None}

  squares_generator = (x**2 for x in range(10))

  unique_names = {user.name for user in users if user is not None}

  eat(jelly_bean for jelly_bean in jelly_beans
      if jelly_bean.color == 'black')
```
错误的：
```python3
  result = [complicated_transform(
                x, some_argument=x+1)
            for x in iterable if predicate(x)]

  result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

  return ((x, y, z)
          for x in xrange(5)
          for y in xrange(5)
          if x != y
          for z in xrange(5)
          if y != z)
```
### 2.8 默认迭代器和运算符
对某些类型使用默认迭代器和运算符，比如列表，字典和文件。
#### 2.8.1 定义
容器类型，比如列表，字典，定义默认迭代器和归属关系运算符（"in" 或者"not in"）
#### 2.8.2 优点
默认迭代器和运算符比较简单也高效。直接表达运算，不需要其他函数调用。一个函数会经常使用默认的运算符。可以被用到任何一个类型，只要这个类型支持这个运算。
#### 2.8.3 缺点
不能通过函数名称知道对象类别。（比如，has_key()表明这是一个字典类型）。这也可以作为一个优点。
#### 2.8.4 使用
对某些类型使用默认迭代器和运算符，比如列表，字典和文件。内设类型也定义迭代器。除非必要，否则不要使用像在python2中具体迭代函数，比如``dict.iter*()``。
正确的：<br>
```python3
for key in adict: ...
if key not in adict: ...
if obj in alist: ...
for line in afile: ...
for k, v in adict.items(): ...
for k, v in six.iteritems(adict): ...
```
错误的：<br>
```python3
for key in adict.keys(): ...
if not adict.has_key(key): ...
for line in afile.readlines(): ...
for k, v in dict.iteritems(): ...
```
### 2.9 生成器
根据需要使用生成器
#### 2.9.1 定义
生成器函数会产生一个迭代器，这个迭代器每次``yield``(产出)一个值时需要调用``yield``语句。产出这个值之后，生成器函数会暂停直到需要产出下一个值。
#### 2.9.2 优点
代码更简洁。使用生成器比直接使用列表存所有的值要省内存。
#### 2.9.3 使用
使用``yield``来返回值而不用``return``。
### 2.10 Lambda 函数
可以用在one-liners。
#### 2.10.1 定义
lambda在表达式中定义无名函数，而不是在语句中。在高阶函数例如``map()``或者``filter()``定义运算符或回调函数时经常会被使用。
#### 2.10.2 优点
方便
#### 2.10.3 缺点
比普通函数更难理解和查错。由于缺少名称，堆栈跟踪会更加难以理解。函数只包含表达式所以它的表现是很局限的。
#### 2.10.4 使用
可以用在one-liners。如果lambda函数内部代码超过60-80个词，最好还是定义为普通的[嵌套函数](#26-内部或嵌套或本地的类和函数)。
常见的运算比如乘法运算，使用``operator``模块中的函数而不用lambda函数。比如，使用``operator.mul``，而不用``lambda x, y: x * y``。
### 2.11 条件表达式
可以用在one-liners。
#### 2.11.1 定义
条件表达式，有时又叫三元运算符，为``if``语句提供了更短的语法。比如``x = 1 if cond else 2``。
#### 2.11.2 优点
比``if``语句更短更方便。
#### 2.11.3 缺点
可能比``if``语句难理解。如果表达式很长的话，条件可能很难弄懂。
#### 2.11.4 使用
可以用在one-liners。其他情况下，更推荐使用完整的``if``语句。
### 3.16 命名
#### 3.19.12 类型注解的导入
