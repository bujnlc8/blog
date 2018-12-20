---
title: mysql存储ip地址的一种方式
date: 2018-12-20 22:26:13
tags:
    - mysql
---

在mysql中，数字的比较远比字符串的比较要来的快，能存数字的就尽量不要存字符串。今天记录一下在mysql中将ip地址存储为数字的一种方式。主要用到mysql自带的两个函数。

<!--more-->

## inet_aton
这个函数将ip地址转换成数字

```sql
mysql> select inet_aton('192.168.1.1');
+--------------------------+
| inet_aton('192.168.1.1') |
+--------------------------+
|               3232235777 |
+--------------------------+
1 row in set (0.00 sec)
```

上面的计算方式其实是`192*(256**3) + 168*(256**2) +1*256+1`，加起来刚好是3232235777。

## inet_ntoa
这个函数将数字转换成ip地址

```sql
mysql>  select inet_ntoa(3232235777);
+-----------------------+
| inet_ntoa(3232235777) |
+-----------------------+
| 192.168.1.1           |
+-----------------------+
1 row in set (0.00 sec)
```
计算方式其实就是上面的计算方式逆转过来，可以从左到右算出每一位：
`3232235777 // (256**3) = 192`，然后用余去除`256**2`以此类推。当然也可以从右到左算出每一位：`3232235777 % 256=1`,然后`3232235777 / 256 % 256 `算出第三位，以此类推。
## ipv6版本
上面的这两个函数只支持ipv4地址，相应的ipv6版本是[`inet6_aton`](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet6-aton)和[`inet6_ntoa`](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet6-ntoa)。不再赘述。

## 参考文档
[https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet-aton](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet-aton)




