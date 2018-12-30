# Google python Coding Style Guide - Google python编程规范指南
译者：[Emily-Tang](https://github.com/Emilylulu)<br>
转载请注明出处。<br>
欢迎PR。 - Pull Request is welcomed!<br>
# 目录
1. [背景](#1-背景)<br>
2. [python语言规则](#2-python语言规则)<br>
2.1 [Lint](#21-Lint)<br>
2.1.1 [定义](#211-定义)<br>
2.1.2 [优点](#212-优点)<br>
2.1.3 [缺点](#213-缺点)<br>
2.1.4 [使用](#214-使用)<br>
2.2 [导入模块](#22-导入模块)<br>
2.2.1 [定义](#221-定义)<br>
2.2.2 [优点](#222-优点)<br>
2.2.3 [缺点](#223-缺点)<br>
2.2.4 [使用](#224-使用)<br>
2.3 [包](#23-包)<br>
2.3.1 [优点](#231-优点)<br>
2.3.2 [缺点](#232-缺点)<br>
2.3.3 [使用](#233-使用)<br>
3.19.12 [类型注解的导入](#31912-类型注解的导入)<br>
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
导入应该像下面的例子中一样
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
#### 3.19.12 类型注解的导入
