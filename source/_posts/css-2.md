---
title: css(2)
date: 2016-06-04 21:36:10
tags: CSS
categories: web前端
---

## @import 指令
与link类似，@import用于指示Web浏览器加载个外部样式表，并在表现HTML 文档时使用其样式。唯一的区别在于命令的具体语法和位置。可以看到，@import:出现在style容器中。它必须放在这里，也就是要放在其他CSS规则之前，否则将根本不起作用。

``` CSS 
<style type="text/css">
@import url(styles.css);/* @import comes first */
h1 {color:gray;}
</style>
```
与link—样，可以限制所导入的样式表应用于一种或多种媒体，可以在样式表的URL 之后列出要应用此样式的媒体：
```CSS
@import url(sheet2.css) all;
@import url(blueworld.css) screen;
@imporc url(zany.css) projection, print;
```
，CSS要求@import指令出现在样式表中的其他规则之前。如果一个@import出现在其他规则（如body {color:red;}）之后，兼容用户代理会将其忽略。

## 内联样式
一个内联style属性中只能放个声明块，而不能放整个样式表。因此，不能在style属性中放@import，也不能包含完整的规则。style属性的值中只能是规则中出现在大括号之间的部分。

## 声明和关键字
CSS关键字往往由空格分隔。只有一种情况例外。在CSS的font属性中。只有一种情况下可以使用斜线（/）来分隔两个特定关键字。下面是一个例子：
`h2 {font: large/150% sans-serif;}`
斜线分隔了用来设置元素的字体大小和行高的两个关键字。只有在这里才允许font声明中出现斜线。font允许的所有其他关键字都用空格分隔。

## 多类选择器

所谓多选择器就是将多个类合并在一起来共同决定这个元素的样式，比如：
``` CSS 
.important.warning{
    background:red;
}
```
类名之间没有空格，要与之完全匹配的元素的类却要加上空格，如匹配一个p元素:
`<p class="important warning"></p>`
这个样式也能匹配`<p class="urgent important warning"></p>`。
而对于以上的p元素,也`会匹配单独对.important或.warning或.urgent的样式申明`，例子见[CSS 类选择器详解](http://www.w3school.com.cn/css/css_selector_class.asp)。

