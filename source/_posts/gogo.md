---
title: gogo
date: 2018-11-12 22:32:08
tags:
    - Go
    - language
categories: 后端
---

1. >简短变量声明语句只有对已经在同级词法域声明过的变量才和赋值操作语句等价，如果变量是在外部词法域声明的，那么简短变量声明语句将会在当前词法域重新声明一个新的变量。我们在本章后面将会看到类似的例子。

```go
var A string

func initA()  {
	A:="100" // 一个新的变量，不会给包变量A赋值
	fmt.Println("A in init A:", A)
}

func main()  {
	initA()
	fmt.Println("A inner main:", A)
}
output:
A in init A: 100
A inner main: 
```

<!--more-->

2. >在Go语言中，返回函数中局部变量的地址也是安全的。例如下面的代码，调用f函数时创建局部变量v，在局部变量地址被返回之后依然有效，因为指针p依然引用这个变量。
```go
var p = f()

func f() *int {
	v := 1
	return &v
}

func main()  {
	fmt.Println(p, &p, *(&p), *p)
}
output:
0xc000014068 0x11626a8 0xc000014068 1
```
