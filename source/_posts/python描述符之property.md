---
title: python描述符之property
date: 2019-02-17 12:08:52
tags:
    - Python
---

python的描述符[descriptor](https://docs.python.org/2/howto/descriptor.html)是实现[property](https://docs.python.org/release/2.6/library/functions.html#property)背后的原理。

只要某个对象实现了`__get__()`,`__set__()`,`__delete__()`中的任意一个或以上的方法，那么它就是一个描述符(descriptor)。python官方文档表述如下:
> In general, a descriptor is an object attribute with “binding behavior”, one whose attribute access has been overridden by methods in the descriptor protocol. Those methods are __get__(), __set__(), and __delete__(). If any of those methods are defined for an object, it is said to be a descriptor.


下面用python实现property的效果。

<!--more-->

```python
# coding=utf-8
class Property(object):
	"""
	Site: https://docs.python.org/2/howto/descriptor.html
	data descriptors always override instance dictionaries.
	non-data descriptors may be overridden by instance dictionaries.
	"""
	def __init__(self, fget, fset=None, fdel=None):
		self.fget = fget
		self.fset = fset
		self.fdel = fdel
	
	def __get__(self, obj, obj_type):
		print "__get__"
		return self.fget(obj)
	
	def __set__(self, obj, val):
		print "__set__"
		if self.fset is None:
			raise AttributeError("cannot set attribute")
		self.fset(obj, val)
	
	def __delete__(self, obj):
		print "__delete__"
		if self.fdel is None:
			raise AttributeError("cannot delete attribute")
		self.fdel(obj)

	def setter(self, fset):
		print "setter"
		return Property(self.fget, fset, self.fdel)
	
	def deleter(self, fdel):
		print "deleter"
		return Property(self.fget, self.fset, fdel)

class BaseTest(object):
	def __init__(self):
		self.p = "init data"

class TestProperty(BaseTest):
	@Property
	def p(self):
		return "a property named p"
	
	# p = p.sett(p)
	@p.setter
	def p(self, val):
		self._p = val
	
	@p.deleter
	def p(self):
		del self._p
		self.p = None
	
	# 重写__getattribute__会阻止描述器的自动调用
#	def __getattribute__(self, key):
#		return "rewrite value of key"

if __name__ == "__main__":
	t = TestProperty()
	"""
	首先 p = Property(p), 将p传给MyProperty的第一个参数fget,
	此时p是一个none-data property, 执行t.p时会自动调用__get__方法,
	转化成type(t).__dict__["p"].__get__(t, type(t))，也即__get__(p, t, type(t))
	如果是TestProperty.p， 转化成TestProperty.__dict__["p"].__get__(None, TestProperty), 
	也即__get__(p, None, TestProperty)
	分析与下面的实际结果相同
	"""
	print t.__dict__
	print t.p
	print type(t).p
	t.p = 100
	print t.p
	TestProperty.p = 200
	print t.p
	del t.p
	print t.p

```

代码输出如下:
```
setter
deleter
__set__
{'_p': 'init data'}
__get__
a property named p
__get__
a property named p
__set__
__get__
a property named p
200
Traceback (most recent call last):
  File "myproperty.py", line 80, in <module>
    del t.p
AttributeError: p

```