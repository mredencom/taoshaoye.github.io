---
layout: post
title: 数据结构-链表操作-单向链表
category: DataStructure
tags: [DataStructure]
---
数据结构中的链表基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取数组的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 1、 场景 

### 1.1、 中文描述

数据结构中的数组基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取数组的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 2、 代码示例

### 2.1、 链表的节点结构

```golang
//定义一个链表的节点结构
type ListNode struct {
	next  *ListNode
	value interface{}
}

```
### 2.2、 链表的结构

```golang
//构造一个链表的结构
type LinkedList struct {
	head   *ListNode
	length uint
}
```

### 2.3、 创建一个节点

```golang
func NewListNode(v interface{}) *ListNode {
	return &ListNode{nil, v}
}
```

### 2.4、 获取下一个元素

```golang
//获取下一个元素
func (this *ListNode) GetNext() *ListNode {
	return this.next
}
```

### 2.5、 当前的节点值

```golang
//取当前的节点值
func (this *ListNode) GetValue() interface{} {
	return this.value
}
```

### 2.6、 建立一个链表结构

```golang
//建立一个链表结构
func NewLinkedList() *LinkedList {
	return &LinkedList{NewListNode(0), 0}
}
```

### 2.7、 在末一个节点后面插入一个节点

```golang
//在末一个节点后面插入一个节点 返回bool
func (this *LinkedList) InsertAfter(p *ListNode, v interface{}) bool {
	//判断该节点是否存在
	if p == nil {
		return false
	}
	newNode := NewListNode(v)
	oldNext := p.next
	p.next = newNode
	//把旧的节点移到新的节点下一个元素
	newNode.next = oldNext
	//增加节点长度
	this.length++
	return true
}
```

### 2.8、 插入当前节点的前置节点

```golang
//插入当前节点的前置节点 返回bool
func (this *LinkedList) InsertBefore(p *ListNode, v interface{}) bool {
	if nil == p || p == this.head {
		return false
	}
	cur := this.head.next
	pre := this.head
	for cur != nil {
		if cur == p {
			break
		}
		pre = cur
		cur = cur.next
	}
	if cur == nil {
		return false
	}
	newNode := NewListNode(v)
	pre.next = newNode
	newNode.next = cur
	this.length++
	return true
}
```

### 2.9、 打印链表

```golang
func (this *LinkedList) Print() {
	//取头节点
	cur := this.head.next
	format := ""
	//判断当前节点下一个节点是否为nil
	for nil != cur {
		format += fmt.Sprintf("%v", cur.GetValue())
		cur = cur.next
		if nil != cur {
			format += "<-"
		}
	}
	fmt.Println(format)
}
```
### 2.10、 在链表头部插入节点

```golang
//在链表头部插入节点 返回 bool
func (this *LinkedList) InsertToHead(v interface{}) bool {
	return this.InsertBefore(this.head, v)
}
```

### 2.11、 在链表尾部插入节点

```golang
func (this *LinkedList) InsertToTail(v interface{}) bool {
	cur := this.head
	//循环找到链表的尾部
	for cur.next != nil {
		cur = cur.next
	}
	//最后插入尾部
	return this.InsertAfter(cur, v)
}
```

### 2.12、 指定索引查找对应节点

```golang
//给出指定索引查找对应节点
func (this *LinkedList) FindByIndex(index uint) *ListNode {
	if index >= this.length {
		return nil
	}
	//取的第一个节点
	cur := this.head.next
	var i uint = 0
	for ; i < index; i++ {
		cur = cur.next
	}
	return cur
}
```

### 2.13、 删除的传入的节点

```golang
//删除的传入的节点 返回bool
func (this *LinkedList) DeleteNode(p *ListNode) bool {
	//判断节点是否存在
	if p == nil {
		return false
	}
	//取当前节点
	cur := this.head.next
	//头节点  就是我们叫的前置节点
	pre := this.head
	//循环链表
	for cur != nil {
		//找到了删除节点
		if p == cur {
			break
		}
		pre = cur
		cur = cur.next
	}
	//当前节点==nil 就没有找到要删除的节点
	if cur == nil {
		return false
	}
	//做删除操作  就是把前置节点下一个节点 替换掉
	pre.next = p.next
	//然后释放掉删除的几点
	p = nil
	//减少链表的长度
	this.length--
	return true
}
```

## 3、 测试源码

测试方法我上面都追加有测试的命令.可以测试使用

```golang
//go test -v -run TestLinkedList_InsertAfter -o singlelinkedlist_test.go
//结果:9->8->7->6->5->4->3->2->1
func TestLinkedList_InsertAfter(t *testing.T) {
	l := NewLinkedList()
	for i := 1; i < 10; i++ {
		l.InsertAfter(l.head, i)
	}
	l.Print()
}

//go test -v -run TestLinkedList_InsertBefore -o singlelinkedlist_test.go
//结果:9999->9->8->7->6->5->4->3->2->1
func TestLinkedList_InsertBefore(t *testing.T) {
	l := NewLinkedList()
	for i := 1; i < 10; i++ {
		l.InsertAfter(l.head, i)
	}
	l.InsertBefore(l.head.next, 9999)
	l.InsertBefore(l.head.next, 8888)
	//t.Log(l.head, l.head.next)
	l.Print()
}

//go test -v -run TestLinkedList_FindByIndex -o singlelinkedlist_test.go
func TestLinkedList_FindByIndex(t *testing.T) {
	l := NewLinkedList()
	for i := 1; i < 10; i++ {
		l.InsertAfter(l.head, i)
	}
	l.InsertBefore(l.head.next, 9999)
	l.InsertBefore(l.head.next, 8888)
	t.Log(l.head.next)
	t.Log(l.FindByIndex(0))
	t.Log(l.FindByIndex(20))
}

//go test -v -run TestLinkedList_DeleteNode -o singlelinkedlist_test.go
//删除节点测试
func TestLinkedList_DeleteNode(t *testing.T) {
	l := NewLinkedList()
	for i := 1; i < 3; i++ {
		l.InsertToTail(i)
	}
	//初始打印
	l.Print()
	//第一次删除打印
	t.Log(l.DeleteNode(l.head.next))
	l.Print()
	//第二次删除打印 (这个地方应该是删除失败的.经过第一次删除,第二次删除已经没有第二个节点了.所以会删除失败的)
	t.Log(l.DeleteNode(l.head.next.next))
	l.Print()
}

//go test -v -run TestLinkedList_InsertToTail -o singlelinkedlist_test.go
//测试插入节点尾部
//结果:1<-2<-3<-4<-5<-6<-7<-8<-9<-10<-11<-12<-13<-14<-15<-16<-17<-18<-19<-20<-21<-22<-23<-24<-25<-26<-27<-28<-29
func TestLinkedList_InsertToTail(t *testing.T) {
	l := NewLinkedList()
	for i := 1; i < 30; i++ {
		l.InsertToTail(i)
	}
	l.Print()
}
```


## 4、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/linkedlist/singlelinkedlist.go)

* [点击查看测试源码](https://github.com/selfjt/algorithm/blob/master/golang/linkedlist/singlelinkedlist_test.go)

* [喜欢可以star下](https://github.com/selfjt/algorithm)
