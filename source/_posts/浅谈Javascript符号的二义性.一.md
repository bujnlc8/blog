---
title: 浅谈Javascript符号的二义性(一)
date: 2016-05-10 21:34:16
tags: 
  - Javascript
categories: 
  - web前端

---
## 缘起
想写这篇博客的缘由是看了一篇老外关于Javascript的博客[Let's Talk About JavaScript](https://blog.mariusschulz.com/2016/04/25/lets-talk-about-javascript),里面提出了四个问题，即输入以下代码，输出值分别是多少?

``` javascript
[] + [] // ""
[] + {} // "[object Object]"
{} + [] // 0
{} + {} // NaN
```
不看注释后面的答案，我估计很多朋友都得犯混，其实这里面既关乎到js引擎的解析问题，也关乎到运算符的二义性问题，这也是我一直很困或的地方，感兴趣的朋友不妨将此文一读。下面就谈一谈我对javascript中运算符二义性的理解。

<!-- more -->

## 漫谈运算符二义性

###  "+"的二义性
 单个加号作为运算符主要有三种作用，它可以表示字符串的连接，例如
``` javascript
var str = "hello" + "world";
```
或者表示数字取正值的一元运算符，如:
``` javascript
var n = 10;
var n2= +n;
```
或者作为数值表达式的求和运算，如：
``` javascript
var n =1;
var n2 = n+100;
```
三种表示方法中，字符串连接和数字求和是最容易出现符号二义性问题的，因为javascript对这两种运算的处理将依赖于数据类型，这就比较尴尬了，因为对于给定的一个表达式
``` javascript
a=a+b;
```
我们根本无法获知它的真正含义是什么。其实有一条规则，即**如果表达式中存在字符串，则优先按字符串的连接来运算,无论字符串是在加号的左边还是右边**，例如：
```javascript
var v1 = "123"
var v2 = 456
console.log(v1+v2) //"123456"
```
那如果不存在字符串又如何处理呢？
``` javascript
 var o = {};
 var a = 123;
 1.console.log(123+o) // "123[object Object]"
 2.console.log(a+null) // "123"
 3.console.log(null+null) // 0
 4.console.log(undefined+a) // NAN
 5.console.log(a+true) // 124
 6.console.log(a+false) // 123
 7.console.log(true+false) // 1
 8.console.log(true+o) // "true[object Object]"
 9.console.log(null+undefined) // NAN
 10.console.log(undefined+o) // "undefined[object Object]"
```
从上面的例子中可以总结出这样几条规则：
   
 - 加号运算符存在对象，将优先将对象转换成字符串，然后相加；
 - undefined与任何值(除对象外)相加得到的结果均是NAN;
 - null=>0,true=>1,false=>0;
这几条规则从上到下优先级递减。

---

### “{}”的二义性
大括号有四种作用，四种都是语言的词法符号。
``` javascript
1、标签后的复合语句
mylabel:{}
2、其他语句中的复合语句
if(condition){
//
}else{
//
}
3、复合语句中的表达式
{1,2,3}  // 3
4、声明对象直接量
var obj = {
 v1:1,
 v2:2
} //去掉var obj = 将报错SyntaxError: missing ; before statement
var p = {{v1:1,v2:2,v3:3}} //SyntaxError: invalid property id
//一旦作为赋值表达式出现，则会转换成对象,否则倾向于表现为复合语句的“标签”
```
我们来看一个易混淆的：
``` javascript
if(true){
  v1:100
}
```
由于if后面的语句可能是下面三者之一：

 - 一个单行语句
 - 一个表达式语句
 - 一个由大括号包含起来的复合语句
所以我们无法判断是作为复合语句从而是标签声明还是一个对象声明。
实际上结果是**标签声明**。如果需要是对象声明，则用括号来强制声明表达式运算。
``` javascript
if(true)({
  v1:100
}) // Object { v1: 100 }
```
最后，大括号也是结构化异常处理的语法符号：
``` javascript
try{
}
catch(e){
}
finally{
}
```
这里的大括号并不是复合语句的语法符号，而是结构化异常处理中用来分割代码块的符号。因为如果是复合语句的语法符号，那么可以用单行语句来代替，而下面语句是不能执行的：
``` javascript
try
var i =100
catch(e)
```
说了那么多，回到最初的问题：
``` javascript
[] + [] // ""
[] + {} // "[object Object]"
{} + [] // 0
{} + {} // NaN
```
对于第一个,[]作为对象__(typeof [] =>object)__
存在,调用toString函数转换成""

``` javascript
[].toString() //""
```
对于第二个，与第一个类似。
``` javascript
({}).toString() //"[object Object]"
```
对于第三个，出现在句首的**{}**将被当作复合语句，即空语句，js引擎将其忽略（maybe），"[]"转化成""，而“+”放在一个变量的前面是一种很好地转化成整数的技巧：
``` javascript
var num ="22"
console.log(+num) //22
console.log(+("")) //0
```
对于第四个，第一个“{}”与第三个相同，唯一不同的是：
``` javascript
console.log(+({})) // NaN
```
于是迷题就此揭开啦！
另外，如果这样改动，结果就不一样了。
``` javascript
({}) + {}
// "[object Object][object Object]"

({} + {})
// "[object Object][object Object]"
```


## 总结
今天主要总结了加号和大括号的二义性，脑袋也清楚了些，看来以后得多写写。有空再总结总结其他运算符吧。希望小伙伴们看完后能有点收获，另外我写得不对的地方欢迎指正！



## 参考资料
> [Javascript语言精髓与编程实践][1]
> [Let's Talk About JavaScript][2]




[1]:  "http://www.duokan.com/book/84607" "Javascript语言精髓与编程实践 周爱民"
[2]:  "https://blog.mariusschulz.com/2016/04/25/lets-talk-about-javascript" "Let's Talk About JavaScript"

