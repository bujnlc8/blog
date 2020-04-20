---
title: lua学习之数据类型
date: 2020-04-20 22:10:14
tags:
    - lua
---

今天记录一下lua的数据类型。参考[Runoob.com](https://www.runoob.com/lua/lua-data-types.html)。

<!--more -->


## lua数据类型

> Lua 是`动态类型`语言，变量`不要类型定义`,只需要为变量赋值。 值可以存储在变量中，作为参数传递或结果返回。

> Lua 中有 `8` 个基本类型分别为：`nil`、`boolean`、`number`、`string`、`userdata`、`function`、`thread` 和 `table`。

> 可以使用 type 函数测试给定变量或者值的类型：

```lua
print(type("Hello world"))      --> string
print(type(10.4*3))             --> number
print(type(print))              --> function
print(type(type))               --> function
print(type(true))               --> boolean
print(type(nil))                --> nil
print(type(type(X)))            --> string
```

### nil

> nil 类型表示一种没有任何有效值，它只有一个值 -- nil，例如打印一个没有赋值的变量，便会输出一个 nil 值：

```
> print(type(a))
nil
```

> 对于全局变量和 table，nil 还有一个"删除"作用，给全局变量或者 table 表里的变量赋一个 nil 值，等同于把它们删掉：

```lua
tab1 = { key1 = "val1", key2 = "val2", "val3" }
for k, v in pairs(tab1) do
    print(k .. " - " .. v)
end
 
tab1.key1 = nil
for k, v in pairs(tab1) do
    print(k .. " - " .. v)
end
```

### boolean（布尔）

> boolean 类型只有两个可选值：true（真） 和 false（假），Lua 把 false 和 nil 看作是 false，其他的都为 true，数字 0 也是 true:

*数字0也为true，这点和其他语言有很大的不同啊！！*

```lua
print(type(true))
print(type(false))
print(type(nil))
 
if false or nil then
    print("至少有一个是 true")
else
    print("false 和 nil 都为 false")
end

if 0 then
    print("数字 0 是 true")  -- 打印出这行
else
    print("数字 0 为 false")
end
```

### number（数字）

> Lua 默认只有一种 number 类型 -- `double`（双精度）类型（默认类型可以修改 `luaconf.h`里的定义），以下几种写法都被看作是`number`类型:

```lua
print(type(2))
print(type(2.2))
print(type(0.2))
print(type(2e+1))
print(type(0.2e-1))
print(type(7.8263692594256e-06))
```

以上结果都为`number`。


### string（字符串）

> 字符串由一对双引号或单引号来表示。

这点和`python`很像嘛~~

```
string1 = "this is string1"
string2 = 'this is string1'
string1 == string2 -- true
```

> 也可以用 2 个方括号 "`[[]]`" 来表示"一块"字符串。

这点和跨行注释挺像。`--[[这里是注释]]`

```lua
html = [[
<html>
<head></head>
<body>
    <a href="http://www.runoob.com/">菜鸟教程</a>
</body>
</html>
]]
print(html)

```

> 在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:

```lua
> print("2" + 6)
8.0
> print("2" + "6")
8.0
> print("2 + 6")
2 + 6
> print("-2e2" * "6")
-1200.0
> print("error" + 1)
stdin:1: attempt to perform arithmetic on a string value
stack traceback:
        stdin:1: in main chunk
        [C]: in ?
>
```

> 使用 # 来计算字符串的长度，放在字符串前面；

```lua
> len = "www.runoob.com"
> print(#len)
14
> print(#"www.runoob.com")
14
>
```

### table（表）

> 在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。也可以在表里添加一些数据，直接初始化表:

```lua
-- 创建一个空的 table
local tbl1 = {}
 
-- 直接初始表
local tbl2 = {"apple", "pear", "orange", "grape"}

```

> Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。

> 不同于其他语言的数组把 0 作为数组的初始索引，在 Lua 里表的默认初始索引一般以 1 开始。

这点很异类啊!

> table 不会固定长度大小，有新数据添加时 table 长度会自动增长，没初始的 table 都是 nil。

### function（函数）

> 在 Lua 中，函数是被看作是"第一类值（First-Class Value）"，函数可以存在变量里:

```lua
function factorial1(n)
    if n == 0 then
        return 1
    else
        return n * factorial1(n - 1)
    end
end
print(factorial1(5))
factorial2 = factorial1
print(factorial2(5))
```
这点倒是和Python很像！

> function 可以以匿名函数（anonymous function）的方式通过参数传递:

```lua
function testFun(tab,fun)
    for k ,v in pairs(tab) do
        print(fun(k,v));
    end
end

tab={key1="val1",key2="val2"};
testFun(tab,
function(key,val)--匿名函数
        return key.."="..val;
end
);

> 
key1 = val1
key2 = val2

```

### thread（线程）

> 在 Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西。

> 线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停。

什么贵，不太懂🤷‍♂️

### userdata（自定义类型）

> userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。

这个听起来也很厉害的亚子。


