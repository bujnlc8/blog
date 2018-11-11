---
title: '谈谈window.open,document.open及window.location.href的区别'
date:  2016-05-23 22:00:03
tags:  HTML
categories: web前端

---


## 前言
今天在做的项目的时候发现在ios下微信的返回按键失效：点击左上角的返回按钮不会回退到上一页，而是不停地刷新页面！由于这个页面是点击一个链接之后打开的，于是我猜想这会不会与`target`属性的设置有关（即`_blank`,`_top`,`_self`,`_parent`等），进而又想到打开页面的另一种方式`window.location.href`以及与之很相似的`document.open`。觉得它们还是有点区别的，做笔记记录一下。

<!-- more -->

## window.open

首先，window.open是window对象的一个方法，用于打开一个新的浏览器窗口或查找一个已命名的窗口。
语法
>window.open(URL,name,features,replace)

|参数|	描述|
|--|--------|
|URL	|一个可选的字符串，声明了要在新窗口中显示的文档的 URL。如果省略了这个参数，或者它的值是空字符串，那么新窗口就不会显示任何文档。|
|name|	一个可选的字符串，该字符串是一个由逗号分隔的特征列表，其中包括数字、字母和下划线，该字符声明了新窗口的名称。这个名称可以用作标记 `<a>` 和 `<form>` 的属性 target 的值。如果该参数指定了一个已经存在的窗口，那么 open() 方法就不再创建一个新窗口，而只是返回对指定窗口的引用。在这种情况下，features 将被忽略。|
|features|	一个可选的字符串，声明了新窗口要显示的标准浏览器的特征。如果省略该参数，新窗口将具有所有标准特征。在窗口特征这个表格中，我们对该字符串的格式进行了详细的说明。|
|replace|一个可选的布尔值。规定了装载到窗口的 URL 是在窗口的浏览历史中创建一个新条目，还是替换浏览历史中的当前条目。支持下面的值：<br>true - URL 替换浏览历史中的当前条目。<br>false - URL 在浏览历史中创建新的条目。|

上面name属性所指的即是以下5种属性值：

|值	|描述|
|--|------|
|_blank	|在新窗口中打开被链接文档。|
|_self	|默认。在相同的框架中打开被链接文档。|
|_parent|	在父框架集中打开被链接文档。|
|_top	|在整个窗口中打开被链接文档。|
|framename|	在指定的框架中打开被链接文档。|

上面的features属性是描述打开窗口的属性的，主要有以下几个：
> 窗口特征（Window Features）

|属性|含义|
|--|---|
|channelmode=yes&#124;no&#124;1&#124;0	|是否使用剧院模式显示窗口。默认为 no。|
|directories=yes&#124;no&#124;1&#124;0|	是否添加目录按钮。默认为 yes。|
|fullscreen=yes&#124;no&#124;1&#124;0	|是否使用全屏模式显示浏览器。默认是 no。处于全屏模式的窗口必须同时处于剧院模式。|
|height=pixels|	窗口文档显示区的高度。以像素计。|
|left=pixels	|窗口的 x 坐标。以像素计。|
|location=yes&#124;no&#124;1&#124;0	|是否显示地址字段。默认是 yes。|
|menubar=yes&#124;no&#124;1&#124;0	|是否显示菜单栏。默认是 yes。|
|resizable=yes&#124;no&#124;1&#124;0|	窗口是否可调节尺寸。默认是 yes。|
|scrollbars=yes&#124;no&#124;1&#124;0|	是否显示滚动条。默认是 yes。|
|status=yes&#124;no&#124;1&#124;0	|是否添加状态栏。默认是 yes。|
|titlebar=yes&#124;no&#124;1&#124;0	|是否显示标题栏。默认是 yes。|
|toolbar=yes&#124;no&#124;1&#124;0|	是否显示浏览器的工具栏。默认是 yes。|
|top=pixels	|窗口的 y 坐标。|
|width=pixels|	窗口的文档显示区的宽度。以像素计。


## window.location.href

与window.open不同，window.location.href只是window.location的一个属性，来标记当前打开的窗口的链接。给window.location.href赋值相当于将当前页面重定向到新的url，它并不会打开新的页面。

## document.open

document.open其实跟上面说的两个关系不大，只是和window.open容易混淆。
document.open很明显是document的一个方法，它用来打开一个新文档，并擦除当前文档的内容。
> 语法
document.open(mimetype,replace)

|参数|	描述|
|--|---|
|mimetype|	可选。规定正在写的文档的类型。默认值是 "text/html"。|
|replace	|可选。当此参数设置后，可引起新文档从父文档继承历史条目。|

看上面的用法实在看不懂，且看下面的例子：

```html
<html>
<head>
<script type="text/javascript">
function createNewDoc()
  {
  var newDoc=document.open("text/html","replace");
  var txt="<html><body>Learning about the DOM is FUN!</body></html>";
  newDoc.write(txt);
  newDoc.close();
  }
</script>
</head>
<body>
<input type="button" value="Write to a new document"
onclick="createNewDoc()">
</body>
</html>
```
点击`Write to a new document`按钮，整个文档被替换成了`<html><body>Learning about the DOM is FUN!</body></html>`,看起来很流弊的样子有木有！

## 区别

只谈window.location.href和window.open的区别。它们最大的区别其实就是window.open可以打开新窗口，而window.llocation.href只能不停地在原窗口炒冷饭。

## 参考
- [document.open](http://www.w3school.com.cn/jsref/met_doc_open.asp) 
- [window.open](http://www.w3school.com.cn/jsref/met_win_open.asp)
