---
title: '六百字读懂Git[转载]'
date: 2016-05-13 00:38:13
tags: 
      - Git
      - Javascript
categories: git

---

偶然发现一篇好文，摘抄如下：

## 好文

> 译注：来自 Hacker School 的 Mary Rose Cook 实现了一个纯 JavaScript 写就的 Git：[Gitlet](http://gitlet.maryrosecook.com/)，包含了最主要的一些命令。这个项目一是为了了解 Git 内部原理，二是希望写一篇深入浅出解释 Git 核心概念的短文。学习一件东西的原理最好的方法就是去亲自实现它，而设计精巧的 Git 核心功能代码也不过 300 行。这就是这篇精巧的小文：[Git in 600 words](http://maryrosecook.com/blog/post/git-in-six-hundred-words)，相应的代码在 [Github](https://github.com/maryrosecook/gitlet)上。短文很有趣，思路清晰也足够深入，值得一看。

<!-- more -->

> 设想你现在位于 `alpha/` 目录下，这里有一个文本文件 `number.txt`，里面的内容只有一个词：“first”。
现在执行 `git init` 将这个 `alpha` 文件夹初始化为 Git 仓库。

> 执行 `git add number.txt`会将 `number.txt` 添加到 Git 的索引（index）中。这个索引记录了所有 Git 保持追踪的文件，现在它有了一个映射记录 `number.txt -> first`，同时 `add` 命令还会把一个包含了 first 字符串的二进制对象加入 Git 的对象数据库里。

> 现在执行 `git commit -m first`。这条命令会做三件事情。首先在对象数据库内创建一个树对象，用以记录 alpha 目录下的文件列表，这个对象有一个指针指向前面 `git add` 命令创建的 `first` 二进制对象；第二，这条命令还会创建一个 commit 对象用以代表刚刚提交的版本，它包含一个指针指向刚刚的树对象；第三，`master` 分支也会指向这个新创建的 `commit` 对象。

> 现在执行 `git clone . ../beta`。它会创建一个新目录 beta 并将其初始化为 Git 仓库，然后把 alpha 仓库的对象数据库中所有对象拷贝给 beta 的对象数据库，将 `beta` 的 `master` 分支像 `alpha` 的 `master` 一样指向相应的对象。它还根据 `first` 提交的内容配置索引，并根据索引更新目录下的文件——也就是 `number.txt`。

> 现在切换到 beta 目录，修改 `number.txt` 的内容为“second”，执行 `git add number.txt` 和 `git commit -m second`，新创建的提交对象 `second`（译注：姑且称之为 second）会有一个指向父提交（`first`）的指针，表示 `second` 继承自 `first`，而 `master` 分支则指向 `second` 提交。

> 回到 `alpha` 目录，执行 `git remote add beta ../beta`，将 `beta` 仓库设为远程仓库。然后执行 `git pull beta master`。

> 在这条命令背后，它其实会执行 `git fetch beta master`，从 `beta` 仓库中找到 `second` 提交的相关对象拷贝到 `alpha` 仓库；把 `alpha` 中关于 `beta` 的 `master` 分支记录指向这个 `second` 提交；更新 `FETCH_HEAD` 指向刚刚从 `beta` 仓库拉取的 `master` 分支，还是这个 `second` 提交。

> 此外，`pull` 命令还会执行 `git merge FETCH_HEAD`。从 `FETCH_HEAD` 得知最近拉取的分支是 `beta` 仓库的 `master` 分支，据此拿到相应的对象，也就是 `second` 提交对象。此时 `alpha` 的 `master` 分支指着 `first` 提交，正好是 `second` 的祖先提交，于是对于 `merge` 命令来说只需要将 `master` 分支指向 `second`提交即可。接下来 `merge` 命令还会更新索引以匹配 `second` 提交的内容，并且相应更新工作目录中的文件。

> 现在执行 `git branch red`，创建一个名为“red”、指向 `second` 提交的新分支。

> 然后执行 `git checkout red`。在 checkout 之前，HEAD 指向 master 分支，执行命令之后它就指向了 red 分支，使得 red 成为当前分支。

> 接下来把 number.txt 的内容修改为 “third”，执行 `git add numbers.txt` 和 `run git commit -m third`。

> 之后再执行 `git push beta red`，这条命令会把 alpha 仓库内跟 third 提交相关的对象拷贝至 beta 仓库，并且将（alpha 仓库内记录的）beta 仓库 red 分支指向 third 提交。就酱。


## 原文

原文：[六百字读懂 Git](https://segmentfault.com/a/1190000002514702)
