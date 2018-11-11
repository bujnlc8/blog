---
title: python实现单链表
date: 2017-05-05 22:28:48
tags: python
categories: 算法

---

直接上代码...

<!--more-->

```python
#coding=utf-8
#实现一个链表单向链表
class singleLink(object):
	def __init__(self,data=None,n=None):
		self.data =data
		self.next =n
	#生成一个链表
	def createLink(self,datas):
		if len(datas)>0:
			single= singleLink(datas[0],None)
			head = singleLink(None,single)
			s = single
			for o in datas[1:]:
				tmp = singleLink(o)
				s.next=tmp
				s = tmp
			return head
		else:
			return None
	#删除某个节点 t为0删除第一个 1删除所有
	def deleteNode(self,node,t=0):
		up =self
		current = self.next
		while current is not None:
			if current.data==node:
				if current.next==None:
					up.next=None
				else:
					up.next = current.next
					current = current.next
				if t==0:
					break
			else:
				up = up.next
				current = current.next
	#在某个位置插入一个节点
	def insertNode(self,node,position):
		index =1
		next = self
		while index<=position:
			next = next.next
			index+=1
		newNode = singleLink(node,next.next)
		next.next = newNode
					
	#删除第一个节点
	def deleteFirst(self):
		next = self.next
		if next.next is not None:
			self.next = next.next
		else:
			self.next =None
		
	def __str__(self):
		s=""
		tmp =self.next
		while tmp is not None:
			s+=str(tmp.data)+'-->'
			tmp = tmp.next
			if tmp is None:
				s+='null'
		return s
			
		

if __name__=='__main__':
	s = singleLink()
	link = s.createLink([1,2,3,4,2,6,7,1,2,3])
	link.deleteFirst()
	link.deleteNode(2,1)
	print(link)#3-->4-->6-->7-->1-->3-->null
	link.insertNode(100, 2)
	print(link) #3-->4-->100-->6-->7-->1-->3-->null
		



