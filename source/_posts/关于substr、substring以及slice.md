---
title: 关于substr、substring以及slice
date: 2016-05-27 23:27:33
tags: Javascript
categories: web前端

---

## 前言
今天记录一下在JS中很容易混淆的`substr()`、`substring()`以及`slice()`。
<!-- more -->
## 共同点
这三个函数都可以用来截取字符串。
```javascript
var str = "hello"
console.log(str.substr(1,2));//"el"
console.log(str.slice(1,2));//"e"
console.log(str.substring(1,2))//"e"
```
主要来看看它们的不同点。

---

## 不同点

### substr
> 定义和用法

substr() 方法可在字符串中抽取从 start 下标开始的指定数目的字符。
> 语法
stringObject.substr(start,length)

|参数 |    描述|
|----|--------|
|start|必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。|
|length |可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

> 返回值

一个新的字符串，包含从 stringObject 的 start（包括 start 所指的字符） 处开始的 length 个字符。如果没有指定 length，那么返回的字符串包含从 start 到 stringObject 的结尾的字符。 

从上面的描述可以看到，substr返回从开始位置（包括）开始的length个字符。这里有个特例是如果开始位置和长度都是负数会这样。
```JavaScript
var str = "hello";
console.log(str.substr(-1,1));//相当于str.substr(4,1)=>"o"
console.log(str.substr(-1,3));//相当于str.substr(4,3)=>"o"
console.log(str.substr(-1,-2));//""
console.log(str.substr(-2,-1));//""
console.log(str.substr(1,-2));//""
console.log(1,8);//"ello"
```
从上面可以看出，当第二个参数即length为负数时返回的结果都是空字符串。如果长度超过了能返回的最大长度，则返回所有能返回的，就像上面最后一个例子所示。

### slice
> 定义和用法

slice() 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。
> 语法
stringObject.slice(start,end)

|参数 |描述|
|-----|-------|
|start| 要抽取的片断的起始下标。如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。|
|end|   紧接着要抽取的片段的结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。|

> 返回值

一个新的字符串。包括字符串 stringObject 从 start 开始（包括 start）到 end 结束（不包括 end）为止的所有字符。

同时，[Array](http://www.w3school.com.cn/jsref/jsref_slice_array.asp)也有一个叫slice的方法用于截取数组。
从上面可以看到，slice接收的两个参数分别表示截取字符串的开始(包含)和结束位置(不包含)，且都可以是负数！那么如果开始位置大于结束位置会怎样呢？

```Javascript
var str = "hello"
console.log(str.slice(2,1));//""
console.log(str.slice(2,6));//"llo"
console.log(str.slice(-1,-2));//""
console.log(str.slice(-2,-1));//"l"
console.log(str.slice(-8,-9));//""
console.log(str.slice(-9,-8));//""
console.log(str.slice(-8,1));//"h"
```
从上面代码运行的结果可以看出：
1. 如果开始位置大于结束位置，那么结果为空字符串。
2. 如果两个参数都是负数的直接加上`一次`字符的长度,加上依然为负数的话，返回空字符串。
3. 如果一正一负，将负的自动变为0。最后一个例子就是这种情况。

以上三条从上到下的优先级依次递减。

### substring

> 定义和用法

substring() 方法用于提取字符串中介于两个指定下标之间的字符。
> 语法
stringObject.substring(start,stop)

|参数 |描述|
|----|--------|
|start| 必需。一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。|
|stop|可选。一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多 1。如果省略该参数，那么返回的子串会一直到字符串的结尾。|

> 返回值

一个新的字符串，该字符串值包含 stringObject 的一个子字符串，其内容是从 start 处到 stop-1 处的所有字符，其长度为 stop 减 start。

这个方法和slice 很像，都是返回开始到结束的字符串。但是却有很大的不同。
substring可接收的两个参数都必须为非负整数。如果是负数，自动忽略为0。且看下面的代码。
```JavaScript
var str = "hello";
console.log(str.substring(1,2));//"e"
console.log(str.substr(3,1));//"el"
console.log(str.substr(-1,-2));//""
console.log(str.substring(-2,-1));//""
console.log(str.substring(-1,3));//"hel"
```
从上面的结果可以看出，如果参数存在负数，那么会将其转换成0，如果第二个参数比第一个参数小，那么这两个参数会互换位置，保证第一个参数小于第二个参数。这条规则对于转换为0以后的依然适用。
```Javascript
console.log(str.substring(2,-1));//相当于substring(0,2)=>"he"
```

