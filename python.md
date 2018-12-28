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
## 1 背景
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
```dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin```
在``pylint``中，警告都会用象征性的名字命名。如果用象征性的名字不够明确，则可以再加上注释。
用这种方法可以很容易找到被忽视的警告。
你可以得到一系列``pylint``警告，通过：
```pylint --list-msgs```
针对某一条获得更多信息，通过：
```pylint --help-msg=C6409```
可以用``pylint: disable`` 替换掉旧的格式``pylint: disable-msg``
对于没使用过的参数的警告，可以在函数开始删掉对应的变量来忽视这条警告。永远记得要加上一条注释表明为什么要删掉。用``unused``去注释就是很好的选择。例如：
``` python3
def viking_cafe_order(spam, beans, eggs=None)：

    del beans, eggs  # Unused by vikings.
    
    return spam + spam + spam
```
