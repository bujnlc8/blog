---
title: 关于jquery的$.extend()、$.fn和$.fn.extend()
date: 2016-05-30 21:39:00
tags: jquery
categories: 前端
---

## 前言
[jQuery](http://caibaojian.com/jquery/)为开发插件提拱了两个方法，分别是：

jQuery.fn.extend();

jQuery.extend();

<!-- more -->

## jQuery.fn

>jQuery.fn = jQuery.prototype = {
　　　init: function( selector, context ) {//….
//……
};

所以jQuery.fn就是jQuery.prototype的另一种写法。当然也可以写成$.fn。

## jQuery.extend(object)

为jQuery类添加类方法，可以理解为添加`静态方法`。如：

>jQuery.extend({
min: function(a, b) { return a < b ? a : b; },
max: function(a, b) { return a > b ? a : b; }
});
jQuery.min(2,3); //  2 
jQuery.max(4,5); //  5


## Object jQuery.extend( target, object1, [objectN])

用一个或多个其他对象来扩展一个对象，返回被扩展的对象。

>var settings = { validate: false, limit: 5, name: "foo" }; 
var options = { validate: true, name: "bar" }; 
jQuery.extend(settings, options); 
结果：settings == { validate: true, limit: 5, name: "bar" }

## jQuery.fn.extend(object);

对jQuery.prototype进得扩展，就是为jQuery类添加“成员函数”。jQuery类的实例可以使用这个“成员函数”。

比如我们要开发一个插件，做一个特殊的编辑框，当它被点击时，便alert 当前编辑框里的内容。可以这么做：
>$.fn.extend({          
    alertWhileClick:function() {            
          $(this).click(function(){                 
                 alert($(this).val());           
           });           
     }       
});       
$("#input1").alertWhileClick(); 

$("#input1")　为一个jQuery实例，当它调用成员方法 alertWhileClick后，便实现了扩展，每次被点击时它会先弹出目前编辑里的内容。

jQuery.extend() 的调用并不会把方法扩展到对象的实例上，引用它的方法也需要通过jQuery类来实现，如jQuery.init()，而 jQuery.fn.extend()的调用把方法扩展到了对象的prototype上，所以实例化一个jQuery对象的时候，它就具有了这些方法，这 是很重要的，在jQuery.JS中到处体现这一点。

## jQuery.fn.extend = jQuery.prototype.extend

可以拓展一个对象到jQuery的 prototype里去，这样的话就是插件机制了。

>(function( $ ){
$.fn.tooltip = function( options ) {
};
//等价于
var tooltip = {
function(options){
}
};
$.fn.extend(tooltip) = $.prototype.extend(tooltip) = $.fn.tooltip
})( jQuery );

## extend的实现原理
见文章[jQuery中extend的实现原理](https://www.hitoy.org/extend-in-jquery.html)


##参考资料 

>[理解jquery的$.extend()、$.fn和$.fn.extend()](http://caibaojian.com/jquery-extend-and-jquery-fn-extend.html)

