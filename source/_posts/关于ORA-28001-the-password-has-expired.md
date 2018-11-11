---
title: '关于ORA-28001: the password has expired'
date: 2016-06-01 20:40:27
tags: 
      - Oracle
      - 数据库
categories: 数据库
---

## 前言
今天在启动项目的时候在tomcat后台突然报了`ORA-28001: the password has expired`的错，搞得系统也进不去了，郁闷了好久，网上查了才知道是过期了，记录一下。

<!--more-->

## 原因与对策

Oracle提示错误消息`ORA-28001: the password has expired`，是由于Oracle11G的新特性所致， Oracle11G创建用户时缺省密码过期限制是180天（即6个月）， 如果超过180天用户密码未做修改则该用户无法登录。 Oracle公司是为了数据库的安全性默认在11G中引入了这个默认功能，但是这个默认的功能很容易被DBA或者是开发人员给疏忽，一旦密码180天未修改过，就会出现这样的问题。
解决方法如下：

1. 运行SQLPlus命令行工具, 输入:
```sql
connect as sysdba;
```
2. 输入dba的用户名和密码后运行:
```sql
SELECT * FROM dba_profiles WHERE profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME'
```
查询密码的有效期设置，LIMIT字段是密码有效天数。在密码将要过期或已经过期时可通过如下语句进行修改密码，密码修改后该用户可正常连接数据库。
```sql 
ALTER USER 用户名 IDENTIFIED BY 密码 ; 
```
如果想去除180天的密码生存周期的限制可通过如下SQL语句将其关闭
```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED 
```

现在就可以愉快地玩耍了。
