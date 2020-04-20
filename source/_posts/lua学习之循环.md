---
title: lua学习之循环
date: 2020-04-20 23:34:00
tags:
    - lua
---

今天记录一下lua的循环。参考[Runoob.com](https://www.runoob.com/lua/lua-loops.html)。

<!--more -->

> Lua 语言提供了以下几种循环处理方式：

## 循环

|循环类型|描述|
|:-:|:-:|
|while 循环|在条件为 true 时，让程序重复地执行某些语句。执行语句前会先检查条件是否为 true。|
|for 循环|重复执行指定语句，重复次数可在 for 语句中控制。|
|repeat...until|重复执行循环，直到 指定的条件为真时为止|
|循环嵌套|可以在循环内嵌套一个或多个循环语句（`while do ... end;for ... do ... end;repeat ... until;`）|


### while循环

>Lua 编程语言中 while 循环语法：

```lua
while(condition)
do
   statements
end
```

> statements(循环体语句) 可以是一条或多条语句，condition(条件) 可以是任意表达式，在 condition(条件) 为 true 时执行循环体语句。


如:
```lua
a=10
while( a < 20 )
do
   print("a 的值为:", a)
   a = a+1
end
```

### for 循环

> Lua 编程语言中 for语句有两大类：：

- 数值for循环
- 泛型for循环

#### 数值for循环

```lua
for var=exp1,exp2,exp3 do  
    <执行体>  
end  
```

> var 从 exp1 变化到 exp2，每次变化以 exp3 为步长递增 var，并执行一次 "执行体"。exp3 是可选的，如果不指定，默认为1。

```lua
for i=1,f(x) do
    print(i)
end
 
for i=10,1,-1 do
    print(i)
end
```

#### 泛型for循环

> 泛型 for 循环通过一个迭代器函数来遍历所有值，类似 java 中的 foreach 语句。Lua 编程语言中泛型 for 循环语法格式:

```lua
--打印数组a的所有值  
a = {"one", "two", "three"}
for i, v in ipairs(a) do
    print(i, v)
end 
```

> i是数组索引值，v是对应索引的数组元素值。ipairs是Lua提供的一个迭代器函数，用来迭代数组。

```lua
days = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"}  
for i,v in ipairs(days) do  print(v) end

```

### repeat...until 循环

> Lua 编程语言中 repeat...until 循环语句不同于 for 和 while循环，for 和 while 循环的条件语句在当前循环执行开始时判断，而 repeat...until 循环的条件语句在当前循环结束后判断。

```lua
repeat
   statements
until( condition )
```

> 如果条件判断语句（condition）为 false，循环会重新开始执行，直到条件判断语句（condition）为 true 才会停止执行。

```lua
a = 10
--[ 执行循环 --]
repeat
   print("a的值为:", a)
   a = a + 1
until( a > 15 )

```

### 循环嵌套

> Lua 编程语言中允许循环中嵌入循环。以下实例演示了 Lua 循环嵌套的应用。

> Lua 编程语言中 for 循环嵌套语法格式:

```lua
for init,max/min value, increment
do
   for init,max/min value, increment
   do
      statements
   end
   statements
end
```

> Lua 编程语言中 while 循环嵌套语法格式:

```lua
while(condition)
do
   while(condition)
   do
      statements
   end
   statements
end
```

> Lua 编程语言中 repeat...until 循环嵌套语法格式:

```lua
repeat
   statements
   repeat
      statements
   until( condition )
until( condition )
```

> 除了以上同类型循环嵌套外，我们还可以使用不同的循环类型来嵌套，如 for 循环体中嵌套 while 循环。

```lua
j =2
for i=2,10 do
   for j=2,(i/j) , 2 do
      if(not(i%j))
      then
         break
      end
      if(j > (i/j))then
         print("i 的值为：",i)
      end
   end
end

```

## 循环控制语句

> Lua 支持以下循环控制语句：

|控制语句 | 描述|
|:-:|:-:|
|break 语句|退出当前循环或语句，并开始脚本执行紧接着的语句。|
|goto 语句|将程序的控制点转移到一个标签处。|

### break 语句

> Lua 编程语言 break 语句插入在循环体中，用于退出当前循环或语句，并开始脚本执行紧接着的语句。如果你使用循环嵌套，break语句将停止最内层循环的执行，并开始执行的外层的循环语句。

和其他语言是一样的。

```lua
--[ 定义变量 --]
a = 10

--[ while 循环 --]
while( a < 20 )
do
   print("a 的值为:", a)
   a=a+1
   if( a > 15)
   then
      --[ 使用 break 语句终止循环 --]
      break
   end
end
```

### goto 语句

> Lua 语言中的 goto 语句允许将控制流程无条件地转到被标记的语句处。

> 语法格式如下所示：

```lua
goto Label
```

> Label 的格式为：

```lua
:: Label ::
```

```lua
local a = 1
::label:: print("--- goto label ---")

a = a+1
if a < 3 then
    goto label   -- a 小于 3 的时候跳转到标签 label
end
```

*注意上面的goto语句不是返回当前，而是从label往后执行，所以会打印出两条`--- goto label ---`。*

> 有了 goto，我们可以实现 continue 的功能：

```lua
for i=1, 3 do
    if i <= 2 then
        print(i, "yes continue")
        goto continue
    end
    print(i, " no continue")
    ::continue::
    print([[i'm end]])
end
```

```
1	yes continue
i'm end
2	yes continue
i'm end
3	 no continue
i'm end
```