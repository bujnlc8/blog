---
title: 构造jquery对象的7种方法
date: 2016-06-02 23:28:48
tags: jquery
categories: web前端

---
## jquery对象
jquery对象是一个类数组对象，含有length属性和大量的jquery方法。
如图所示：
![jquery对象](http://haihuiling.top/images/jquery.png)

jquery对象是由jQuery()创建的，$()则是它的缩写。根据传入参数的不同，可以分为七种创建方法。

<!--more-->

## jQuery(selector,[context])

这是我们最熟悉的。
如果传入一个字符串参数，jQuery会检查这个字符串是选择器表达式还是HTML代码。如果是选择器表达式，则遍历文档，查找与之匹配的DOM元素，并创建一个包含了这些DOM元素引用的jQuery对象；如果没有元素与之匹配，则创建一个空jQuery对象，其中不包含任何元素，其属性length等于0。

默认情况下，对匹配元素的查找将从根元素document对象开始，即查找范围是整个文档树，不过也可以传入第二个参数context来限定查找范围（本书中把参数context称为“选择器的上下文”，或简称“上下文”）。例如，在一个事件监听函数中，可以像下面这样限制查找范围：
```javascript
$('div.foo').click(function() {
   $('span', this).addClass('bar'); // 限定查找范围
});
```

在这个例子中，对选择器表达式“span”的查找被限制在了this的范围内，即只有被点击元素内的span元素才会被添加类样式“bar”。
如果选择器表达式selector是简单的“#id”，且没有指定上下文context，则调用浏览器原生方法document.getElementById()查找属性id等于指定值的元素；如果是比“#id”复杂的选择器表达式或指定了上下文，则通过jQuery方法.find()查找，因此$('span', this)等价于$(this).find('span')。

## jQuery( html [, ownerDocument] )、jQuery( html, props )

如果传入的字符串参数看起来像一段HTML代码（例如，字符串中含有<tag…>），jQuery则尝试用这段HTML代码创建新的DOM元素，并创建一个包含了这些DOM元素引用的jQuery对象。例如，下面的代码将把HTML代码转换成DOM元素并插入body节点的末尾：
```javascript
$('<p id="test">My <em>new</em> text</p>').appendTo('body');
```

如果HTML代码是一个单独标签，例如，`$('<img/>')`或$`('<a></a>')`，jQuery会使用浏览器原生方法document.createElement()创建DOM元素。如果是比单独标签更复杂的HTML片段，例如上面例子中的`$('<p id = "test">My<em>new</em>text</p>')`,则利用浏览器的innerHTML机制创建DOM元素，这个过程由方法jQuery.buildFragment()和方法jQuery.clean()实现。

第二个参数ownerDocument用于指定创建新DOM元素的文档对象，如果不传入，则默认为当前文档对象。
如果HTML代码是一个单独标签，那么第二个参数还可以是props，props是一个包含了属性、事件的普通对象；在调用document.createElement()创建DOM元素后，参数props会被传给jQuery方法.attr()，然后由.attr()负责把参数props中的属性、事件设置到新创建的DOM元素上。

参数props的属性可以是任意的事件类型（如“click”），此时属性值应该是事件监听函数，它将被绑定到新创建的DOM元素上；参数props可以含有以下特殊属性：
`val、css、html、text、data、width、height、offset`，相应的jQuery方法：`.val()、.css()、.html()、.text()、.data()、.width()、.height()、.offset()`将被执行，并且属性值会作为参数传入；其他类型的属性则会被设置到新创建的DOM元素上，某些特殊属性还会做跨浏览器兼容（如type、value、tabindex等）；可以通过属性名class设置类样式，但要用引号把class包裹起来，因为class是JavaScript保留字。例如，在下面的例子中，创建一个div元素，并设置类样式为“test”、设置文本内容为“Click me!”、绑定一个click事件，然后插入body节点的末尾，当点击该div元素时，还会切换类样式test：
```javascript
$("<div/>", {
   "class": "test",
   text: "Click me!",
   click: function(){
     $(this).toggleClass("test");
   }
}).appendTo("body");
```

## jQuery( element )、jQuery( elementArray )

如果传入一个DOM元素或DOM元素数组，则把DOM元素封装到jQuery对象中并返回。

这个功能常见于事件监听函数，即把关键字this引用的DOM元素封装为jQuery对象，然后在该jQuery对象上调用jQuery方法。例如，在下面的例子中，先调用$(this)把被点击的div元素封装为jQuery对象，然后调用方法slideUp()以滑动动画隐藏该div元素：
```
$('div.foo').click(function() {
   $(this).slideUp();
});
```

## jQuery( object )

如果传入一个普通JavaScript对象，则把该对象封装到jQuery对象中并返回。

这个功能可以方便地在普通JavaScript对象上实现自定义事件的绑定和触发，例如，执行下面的代码会在对象foo上绑定一个自定义事件custom，然后手动触发这个事件，执行绑定的custom事件监听函数，如下所示：
```
// 定义一个普通 JavaScript 对象
var foo = {foo:'bar', hello:'world'};
// 封装成 jQuery 对象
var $foo = $(foo);
// 绑定一个事件
$foo.on('custom', function (){
       console.log('custom event was called');
});
// 触发这个事件
$foo.trigger('custom');   // 在控制台打印"custom event was called"
```

## jQuery( callback )

如果传入一个函数，则在document上绑定一个ready事件监听函数，当DOM结构加载完成时执行。ready事件的触发要早于load事件。ready事件并不是浏览器原生事件，而是DOMContentLoaded事件、onreadystatechange事件和函数doScrollCheck()的统称。

## jQuery( jQuery object )

如果传入一个jQuery对象，则创建该jQuery对象的一个副本并返回，副本与传入的jQuery对象引用完全相同的DOM元素。

## jQuery()

如果不传入任何参数，则返回一个空的jQuery对象，属性length为0。注意，在jQuery 1.4之前，会返回一个含有document对象的jQuery对象。

这个功能可以用来复用jQuery对象，例如，创建一个空的jQuery对象，然后在需要时先手动修改其中的元素，再调用jQuery方法，从而避免重复创建jQuery对象。

以上就是构造jquery对象的七种方法，大部分内容来自[《jQuery技术内幕：深入解析jQuery架构设计与实现原理》](https://www.amazon.cn/jQuery%E6%8A%80%E6%9C%AF%E5%86%85%E5%B9%95-%E6%B7%B1%E5%85%A5%E8%A7%A3%E6%9E%90jQuery%E6%9E%B6%E6%9E%84%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86-%E9%AB%98%E4%BA%91/dp/B00J2197XE/ref=sr_1_10?ie=UTF8&qid=1464882886&sr=8-10&keywords=jquery)这本书。
