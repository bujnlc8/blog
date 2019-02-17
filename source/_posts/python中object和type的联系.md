---
title: python中object和type的联系?
date: 2019-02-16 11:04:52
tags:
    - Python
---

1.object和type都是type的一个实例，表示它们都是类型对象。

2.在Python的世界中，object是父子关系的顶端，所有的数据类型的父类都是它；

3.type是类型实例关系的顶端，所有对象都是它的实例。

<!--more -->

4.它们两个的关系可以这样描述：

- object是一个type，object is an instance of type。即object是type的一个实例。object 无父类，因为它是链条顶端。

```python
In [1]: object.__class__
Out[1]: type
In [2]: object.__bases__
Out[2]: ()
```

- type是一种object， type is kind of object。即type是object的子类。type的类型是自己。

```python
In [3]: type.__bases__
Out[3]: (object,)
In [4]: type.__class__
Out[4]: type
```

4.关系如下图所示:

![object and type](https://ws3.sinaimg.cn/large/006tKfTcgy1g082x69zh3j30q60eu75z.jpg)

5.参考文档:

1.[Python’s object model explained](http://blog.invisivel.net/2012/04/10/pythons-object-model-explained/)

2.[Python 的 type 和 object 之间是怎么一种关系？](https://www.zhihu.com/question/38791962)

3.[Objects, values and types](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types)
