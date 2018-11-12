---
title: Python 2 与 Python 3 的差异对比
date: 2016-10-20 00:10:08
tags: 
    - Python
    - language
categories: 后端
---

# Python 2 与 Python 3 的差异对比
## 1.Print是一个函数
在Python3中print是个函数，这意味着在使用的时候必须带上小括号,并且它是带有参数的。

```python
    old: print "The answer is", 2+2
    new: print("The answer is", 2+2)  
    old: print x,      # 末尾加上逗号阻止换行  
    new: print(x, end="") # 使用空格来代替新的一行  
    old: print >> sys.staerr, "fatal error"  
    new: print ("fatal error", file=sys.stderr)  
    old: print (x, y)   # 打印出元组(x, y) 
    new: print((x, y))  # 同上,在python3中print(x, y)的结果是跟这不同的
```

<!-- more -->

在Python3中还可以定义分隔符，使用参数`sep`来指定.

```python
print("There are <", 2+5, ">possibilities", sep="_")
output:There are <_7_>possibilities
```

**print()函数不支持Python2.X中print中的“软空格”。在Python2.X中,print "A\n", "B"的结果是"A\nB\n";而在Python3中print("A\n", "B")的结果是"A\n B\n"。**

## 2.使用Views和Iterators代替Lists
* dict的方法dict.keys(),dict.items(),dict.values()不会再返回列表,而是返回一个易读的“views”。这样一来，像这样的语法将不再有用了:k = d.keys();k.sort(),你可以使用k = sorted(d)来代替。sorted(d)在Python2.5及以后的版本中也有用，但是Python3效率更高了。
  
  ```python
    d = {'a': 1}
    d.keys()     # dict_keys(['a'])  
    d.items()    # dict_items([('a', 1)])  
    d.values()   # dict_values([1])  
    k = d.keys(); k.sort()     # AttributeError: 'dict_keys' object has no attribute 'sort'
```

* 同样，dict.iterkeys(),dict.iteritems(),dict.itervalues()方法也不再支持。
* map()和filter()将返回iterators。如果你真的想要得到列表,list(map(...))是一个快速的方法，但是更好的方法是使用列表推导(尤其是原代码使用了lambda表达式的时候),或者重写原来的代码,改为不需要使用列表。特别是map()会给函数带来副作用，正确的方法是改为使用for循环，因为创建一个列表是非常浪费的事情。
* Python3中的range()函数跟Python2.X的xrange()函数的作用是一样的，这样可以使用任意的数字,Python3中去除了xrange()函数。
* zip()在Python3中返回的是一个迭代器。
## 3.整型数
* 从本质上来说,long重命名了int,因为在内置只有一个名为int的整型,但它基本跟之前的long一样。
* 像1/2这样的语句将返回float，即0.5。使用1//2来获取整型,这也是之前版本所谓的“地板除”。
* 移除了sys.maxint,因为整型数已经没了限制。sys.maxsize可以用来当做一个比任何列表和字符串下标都要大的整型数。
* repr()中比较大的整型数将不再带有L后缀。
* 八进制数的字面量使用0o720代替了0720。
## Text Vs. Data 代替 Unicode Vs. 8-bit
Python3中改变了二进制数据和Unicode字符串。
* Python3使用文本和(二进制)数据的理念代替之前的Unicode字符串和8-bit字符串,所有的文本默认是Unicode编码。使用str类型保存文本,使用bytes类型保存数据。当你混淆文本和数据的时候Python3会抛出TypeError的错误。
* 不能再使用u"..."字面量表示unicode文本,而必须使用b"..."字面量表示二进制数据。
* 因为str和bytes不能弄混,所以你必须显式地将他们进行转换。使用str.encode()将str转换为bytes，使用bytes.decode()将bytes转换为str,也可以使用bytes(s, encoding=...)和str(b, encoding=...)。
* str和bytes都是不可变的类型，有一个分离的可变类型的bytearray可以保存缓存的二进制数据,所有能够接受bytes的API都能够使用bytearray。这些可变的API是基于collections.MutableSequence的。
* 移除了抽象类型basestring，使用str代替。
* 文件默认使用文本类型打开,这也是open()函数默认的。如果要打开二进制文件必须使用b参数,否则会出现错误,而不会默默地提供错误的数据。
* 文件名都使用unicode字符串传入和输出，变量名也是，所以可以使用中文作为变量名，例如 中国="China"; print(中国) 。关于 python2 的编码问题请参考：详解python中文编码与处理 [详解 python 中文编码与处理](http://my.oschina.net/leejun2005/blog/74430)


