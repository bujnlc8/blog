---
title: 关于Javascript中的\x与\u以及\0
date: 2016-05-17 20:40:23
tags: 
     - Javascript
     - Unicode
categories: web前端

---


今天来谈谈Javascript中的`\x`与`\u`以及`\0`。

## 前言
先来看看Javascript的十进制、八进制和十六进制的表示方法：
```javascript
console.log(10);//10
console.log(011);//9 
console.log(0x11);//17
console.log(018==18);//true
console.log(01a);//SyntaxError: identifier starts immediately after numeric literal
console.log(0x1h);////SyntaxError: identifier starts immediately after numeric literal
```
从上面的例子可以看出，如果以0开头的数包含的字符超过了7,但没大于9,则会忽略前面的0,按十进制解析，否则会报错。对于十六进制，如果`0x`后面的字符超过了`f`也会报错。

<!--more -->

另外，Javascript没有自己的的二进制表示方法，但可以通过以下的方式将数字处理成二进制：
```javascript
var a=1111; //2进制数
console.log(parseInt(a,2)===15);//true
```
那么与它们及其相似的`\x`与`\u`以及`\0`是什么东西呢？

----

##  关于`\x`

`\x`是用十六进制表示ascii字符的前缀,对，就只限于ascii字符。且看下面的例子：
```javascript
console.log("\x61");//"a"
console.log("\X61");//"X61"
console.log("\x611");//"a1"
console.log("\x6");//SyntaxError: malformed hexadecimal character escape sequence
console.log("\xgg");//SyntaxError: malformed hexadecimal character escape sequence
```
从上面的例子可以总结出以下结论：

 - `\x`是用十六进制表示ascii字符的前缀，不能是`\X`。
 - `\x`只解析紧接着的两位十六进制，有且只能是两位，少了会报错，多了会忽略，直接以原始字符的style输出,这也就是它为什么只能表示ascii字符的原因。
 - `\x`紧接着的两位字符只能是十六进制里面的字符(与大小写无关)，其他位可以为任意字符。

---
## 关于`\0`
`\0`是用八进制表示ascii字符的前缀。看下面的例子:

```javscript
console.log("\066");//6
console.log(("\066").charCodeAt(0)===54);//true
console.log("\066789");//"6789"
console.log(("\0ab")//"�ab"
console.log("\0");//"�" 应该是空字符
console.log(("\0").charCodeAt(0)===0);//true
console.log("\07");//不可见字符
console.log(("\07").charCodeAt(0)===7);//true
```
从上面的例子中也可以总结出以下结论：

-  `\0`是用八进制表示ascii字符的前缀，可以接受0,1,2...位的数字，不会报错。
-  当`\0`后面没有字符时表示空字符。这点与`\x`不同，`\x`后续没有字符会报错。
-  当`\0`后面有一个字符时将其解释为八进制数，并转换成相应的ascii字符，但是如果的数大于7,会忽略后面的数，直接解析`\0`即得到空字符串，后面的字符串按原来的style输出。
-  当`\0`后面有两个字符时，会试着将两个字符以八进制解释，并转换成相应的ascii字符，但一旦有字符大于7，则按从左到右的顺序依次忽略。看以下的代码，我相信你知道我在说什么。
```javscript
console.log("\066");"6"
console.log("\079");"不可见字符"+"9"
console.log("\088");"空字符"+"88"
```
-  当后面跟上三个或三个以上字符时，先会按照上一条规则解析前两个字符，直接忽略后面的字符。

---

## 关于`\u`

`\u`是Javascript用来表示unicode编码的前缀。我们知道，javascript内部表示字符是用unicode来表示的。如`好`的unicode编码是`597d`，那么可以在Javascript里面用`\u597d`来表示`好`：
```javascript
console.log("\u597d");//"好"
```
关于javascrit和unicode的关系可以参考这篇文章[Unicode与JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html)。

 
 








