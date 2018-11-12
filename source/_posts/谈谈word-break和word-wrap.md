---
title: 谈谈word-break和word-wrap
date: 2016-05-19 19:38:24
tags: 
     - CSS
categories: 前端

---

## 前言

今天在工作的时候发现`div`连续`append`多个a标签后会溢出，最后用`word-break:break-all`和`word-wrap:break-word`强制，它们很容易混淆，花点时间记一下。

<!-- more -->

---

## word-break
word-break是干什么用的呢？顾名思义，把单词断开，也就是对于一个长的单词会把这个单词从某个字母处折断，折断的后半部分移到下一行。
word-break有3个值可选。

|值 |	描述|
|---|---|
|normal |	使用浏览器默认的换行规则。
|break-all| 	允许在单词内换行。
|keep-all| 	只能在半角空格或连字符处换行。

经过我在Chrome和firefox的测试，效果如下表所示：

|---|Chrome|firefox|
|--|--|--|
|normal|长单词溢出，中文正常换行,遇到连字符正常换行|长单词溢出，中文正常换行，遇到连字符正常换行|
|break-all|长单词、中文正常换行|长单词、中文正常换行|
|keep-all|长单词溢出，中文正常换行，遇到连字符正常换行|长单词溢出，、中文正常换行|
由表可以看出，firefox和chrome表现的最大不同在于当word-break为`keep-all`时，这时chrome遇到单词的连字符会换行，而firefox不会。

---

## word-wrap

什么是word-wrap?从字面意思理解，包裹单词，那是什么意思？就是指示容器怎么容纳这个词。
它有两个属性可供选择：

|值 	|描述|
|---|---|
|normal 	|只在允许的断字点换行（浏览器保持默认处理）|
|break-word| 	在长单词或 URL 地址内部进行换行|

从这里我们可以看到，貌似word-wrap和word-break很像啊，那为啥会存在这两个相似的东西呢？

## word-break和word-wrap的区别
先要明确一点，不加word-wrap或word-break的时候，就是浏览器默认的时候，如果有一个单词很长，导致一行中剩下的空间已经放不下它时，则浏览器会把这个单词挪到下一行去。不过万一这个单词的长度已经比一行的长度还长了，那必然会产生溢出了。这时如果加上`word-wrap:break-word`，那么会把这个单词从行尾断开。
那word-break是什么鬼？
如果在我们上面说到的那个长单词加上`word-break:break-all`，那么浏览器不会尝试把长单词移到下一行，而是在当行就换行。从这一点来看，`word-break`明显比`word-wrap`节省空间。

---

## 总结
`word-wrap `是用来决定允不允许单词内断句的，如果不允许的话长单词就会溢出。最重要的一点是它还是会首先尝试挪到下一行，看看下一行的宽度够不够，不够的话就进行单词内的断句。
而`word-break:break-all`则更变态，因为它断句的方式非常粗暴，它不会尝试把长单词挪到下一行，而是直接进行单词内的断句。这也可以解释为什么说它的作用是决定用什么方式来断句的，那就是——用了`word-break:break-all`，就等于使用粗暴方式来断句了。总之一句话，如果您想更节省空间，那就用`word-break:break-all`就对了。

> [你真的了解word-wrap和word-break的区别吗?](http://www.cnblogs.com/2050/archive/2012/08/10/2632256.html)
