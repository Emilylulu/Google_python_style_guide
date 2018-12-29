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
#### 3.19.12 类型注解的导入
