---
title: shell的一些用法
date: 2018-11-12 16:07:13
tags:
    - shell
    - linux
categories:
    - Linux
---

今天记录一下linux shell的一些用法.
<!-- more -->

1. `ls -F` 
2. `LS_COLORS`环境变量？
3. centos-7安装中文语言包
> #yum install kde-l10n-Chinese
#yum reinstall glibc-common

4.`(pwd;ls)` 括号的加入使命令列表变成了进程列表，生成了一个子shell来执行对应的命令。进程列表是一种命令分组（command grouping）。另一种命令分组是将命令放入花括号中，并在命令列表结尾加上分号`;`。如`{command;}`。但是花括号进行命令分组并不会像进程列表那样创建出子shell。`$BASH_SUBSHELL`，如果该值为0，说明没有子shell,如果返回1或者更大的数字，表明存在子shell。
5.`sleep 10 &` 方括号返回后台作业(background job)号和后台作业进程号。
>[linghaihui@centos_1 ~]$ sleep 10 &
>[2] 22686

可以用`jobs`命令显示后台作业信息：
>[linghaihui@centos_1 ~]$ jobs
>[1]-  Running                 sleep 1000 &
>[2]+  Done                    sleep 10

6.协程可以同时做两件事：在后台生成一个子shell，并在这个子shell中执行命令。要进行协程处理，需要用到`coproc`命令。
>[linghaihui@centos_1 ~]$ coproc  sleep 100
[2] 22705
[linghaihui@centos_1 ~]$ jobs 
[1]-  Running                 sleep 1000 &
[2]+  Running                 coproc COPROC sleep 100 &

`COPROC`是coproc命令给这个进程起的名字。可以使用如下语法设置自定义的名字:
**$ coproc job_name { command...; }**

注意，必须确保在第一个`{`和命令之间有一个空格。还必须保证以分号`;`结尾，且分号和`}`之间也必须要有一个空格。其实也可以有多个空格。

7.内建命令和外部命令。
>[linghaihui@centos_1 ~]$ type -a exit
exit is a shell builtin
[linghaihui@centos_1 ~]$ type -a ps
ps is /bin/ps
ps is /usr/bin/ps

如上所示，`exit`就是一个内部命令，而`ps`就是一个外部命令。当外部命令执行时，会fork出一个子进程。而内建命令不需要。
有意思的是，有些命令有多种实现，既可以当做内建命令用，也可以当做外部命令用。对于这种命令，默认使用内建命令方式，指明了对应的路径才会当做外部命令使用。
>[linghaihui@centos_1 ~]$ type -a echo
echo is a shell builtin
echo is /bin/echo
echo is /usr/bin/echo

8.输入`!!` 能够唤出刚刚用过的那条命令来使用。