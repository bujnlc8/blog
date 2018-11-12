---
title: 将Web工程代码合并为一个文件
date: 2016-05-11 19:29:22
tags: Java
categories: 后端

---
## 扯犊子
今天工作的时候突然接收到一个任务:`请提供****平台项目一期（不包含知识将赛系统）的代码，放在WORD文档中,用于申请软件著作权`。申请软件著作权什么鬼？于是赶紧问了一下度娘，度娘告诉我软件著作权是这玩意：
> 计算机软件著作权是指软件的开发者或者其他权利人依据有关著作权法律的规定，对于软件作品所享有的各项专有权利。就权利的性质而言，它属于一种民事权利，具备民事权利的共同特征。

<!-- more -->

虽然看起来很高大上，但和我好像没什么关系。。。不过上级安排的任务还是得完成啊，想想这个项目的代码少说也有10万行，文件更是不计其数，手动合并是不可能的,只能用代码了。<br>其实这个任务就是将一个目录下的指定格式文件合并成一个文件输出。首先当然是遍历这个目录下的所有文件，这个可以采用递归的方法来实现，然后通过文件的追加来合并到一个文件里。思路有了，于是我写出了以下代码：

## code
```java
package haihuiling.test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * 合并多个文件为一个
 * @author haihuiling
 * 
 */
public class Many2One {
	public static void main(String[] args) throws IOException {
		String[] formats = { ".java", ".jsp" };
		copyCode2one("D:\\myBlog", "D:\\myBlog\\code.txt", formats);
	}
	public static void copyCode2one(String sourcePath, String destPath,
			String[] formats) {
		File file = new File(destPath);
		if (file.exists()) {
			file.delete();
		}
		try {
			copyCode2oneControl(sourcePath, destPath, formats);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 将目录里的文件合并为一个
	 * 
	 * @param sourcePath
	 * @param destPath
	 * @param formats
	 * @throws IOException
	 */
	public static void copyCode2oneControl(String sourcePath, String destPath,String[] formats) throws IOException {
		File file = new File(sourcePath);
		if (file.exists()) {
			// 遍历文件
			if (file.isDirectory()) {
				String fileNme[] = file.list();
				for (String p : fileNme) {
					copyCode2oneControl(file.getPath() + "\\" + p, destPath,
							formats);
				}
			} else {// 如果是文件，判断文件的格式
				if (sourcePath.lastIndexOf(".") != -1) {
					String format = sourcePath.substring(sourcePath
							.lastIndexOf("."));
					if (isFormat(format, formats)) {// 如果是指定格式的文件,将文件加到目标文件后面
						System.err.println(sourcePath);
						File file2 = new File(destPath);
						FileInputStream fi = new FileInputStream(file);
						FileOutputStream fo = new FileOutputStream(file2, true);
						// byte[] fileName = sourcePath.getBytes();
						// fo.write(fileName);
						byte[] b = new byte[8];
						while (fi.read(b) != -1) {
							fo.write(b);
						}
						fo.flush();
					}
				}
			}
		} else {
			System.err.println("文件或目录不存在");
		}
	}

	/**
	 * 判断文件格式是否在给定的文件格式中
	 * 
	 * @param format
	 * @param formats
	 * @return
	 */
	public static boolean isFormat(String format, String[] formats) {
		if (null != format && null != formats && formats.length >= 1) {
			for (String f : formats) {
				if (format.equals(f)) {
					return true;
				}
			}
		}
		return false;
	}
}

```
## 问题
代码其实很简单，看一下就明白了，但这里有个问题，在第60行代码左右
```java
byte[] b = new byte[8];
```
如果将8改为512或者更大值的时候会出现文件写入不完整的现象。我想问题应该出现在FileOutputStream.write方法里，查看其源码：
```java
public void write(byte b[]) throws IOException {
        Object traceContext = IoTrace.fileWriteBegin(path);
        int bytesWritten = 0;
        try {
            writeBytes(b, 0, b.length, append);
            bytesWritten = b.length;
        } finally {
            IoTrace.fileWriteEnd(traceContext, bytesWritten);
        }
    }
```
关键的是在第5行，但其源码却让人郁闷：
```java
private native void writeBytes(byte b[], int off, int len, boolean append) throws IOException;
```
竟然被声明为**native**,也就是说这个方法是用C语言实现然后加载到java虚拟机的，于是想从源码找出原因是无望了，在网上也查找不到原因。
参考了这篇博客[Java追加文件内容的三种方法](http://blog.csdn.net/malik76/article/details/6408726/),采用了里面提到的三种方法，但值很大的时候也会出现文件写入不完整的情况。略郁闷。

## 参考资料
> [Java追加文件内容的三种方法][1]

[1]: http://blog.csdn.net/malik76/article/details/6408726/ "Java追加文件内容的三种方法"

