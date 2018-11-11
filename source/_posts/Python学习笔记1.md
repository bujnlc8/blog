---
title: Python学习笔记1
date: 2016-05-30 00:35:46
tags: Python
categories: 
            - 编程语言
            - 后端
---
## 零零星星
1. 下划线（`-`）在解释器中有特别的含义，表示最后一个表达式的值。
```Python 
>>> str = "hello"
>>> _  # "hello"
```
2. print在2.0版本之后支持重定向到文件。
```Python 
>>> log = open('log.txt','w')
>>> print >> log,'重定向到文件' #在当前目录出现log.txt文件
```
3. input()与raw_input()
这两个均是 python的内建函数，通过读取控制台的输入与用户实现交互。但它们的功能不尽相同。在python 2.7中两种方法均支持，在python 3.5中只支持input。在2.7中，这两个函数均能接收字符串 ，但raw_input()直接读取控制台的输入（任何类型的输入它都可以接收）。而对于 input()，它希望能够读取一个合法的 python 表达式，即你输入字符串的时候必须使用引号将它括起来，否则它会引发一个 SyntaxError。
4. `'''`
支持跨行输出。`"""`也可以实现。

