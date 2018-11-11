---
title: python dict()用法
date: 2016-10-18 00:49:30
tags: Python
categories: web后端
---

## 包含双值元组的列表
```python
lot = [('a','b'),('c','d'),('e','f')]
dict(lot)
=> {'a': 'b', 'c': 'd', 'e','f'}
```



## 包含双值列表的列表
```python
lol = [['a','b'],['c','d'],['e','f']]
dict(lol)
=> {'a': 'b', 'c': 'd', 'e','f'}
```


## 包含双值列表的元组
```python
tol =(['a','b'],['c','d'],['e','f'])
dict(tol)
=> {'a': 'b', 'c': 'd', 'e','f'}
```

## 包含双值元组的元组
```python
tot=(('a','b'),('c','d'),('e','f'))
dict(tot)
=> {'a': 'b', 'c': 'd', 'e','f'}
```

## 双字符的列表
```python
los=['ab','cd','ef']
dict(los)
=> {'a': 'b', 'c': 'd', 'e','f'}
```

## 双字符的元组
```python
tos=('ab','cd','ef')
dict(tos)
=> {'a': 'b', 'c': 'd', 'e','f'}
```


***最后两种的字符长度只能是2***


