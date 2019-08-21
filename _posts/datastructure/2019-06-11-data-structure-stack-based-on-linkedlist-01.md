---
layout: post
title: 数据结构-栈操作-用链表实现栈基本操作
categories: DataStructure
tags: DataStructure
---
* content
{:toc}

数据结构中的栈基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取栈的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 1、 场景 

### 1.1、 中文描述

数据结构中的栈基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取栈的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 2、 代码示例

### 2.1、 基于链表使用栈的操作

```golang
//使用链表实现栈的结构
//定义一个链表结构
type node struct {
	next *node
	val  interface{}
}
//定义一个栈链表结构
type LinkedListStack struct {
	topNode *node
}
```
### 2.2、 创建一个栈

```golang
//新建一个链表栈
func NewLinkedListStack() *LinkedListStack {
	return &LinkedListStack{nil}
}
```

### 2.3、 判断栈是否是一个空栈

```golang
//判断栈是否为空
func (this *LinkedListStack) IsEmpty() bool {
	if this.topNode == nil {
		return true
	}
	return false
}
```

### 2.4、 向栈添加元素

```golang
//向链表栈push加入数据
func (this *LinkedListStack) Push(v interface{}) {
	this.topNode = &node{this.topNode, v}
}
```

### 2.5、 删除栈元素

```golang
//删除栈顶的元素 并返回栈顶的元素
func (this *LinkedListStack) Pop() interface{} {
	if this.IsEmpty() {
		return nil
	}
	v := this.topNode.val
	//删除元素
	this.topNode = this.topNode.next
	return v
}
```

### 2.6、 取的栈顶的元素

```golang
//取栈顶表的数据
func (this *LinkedListStack) Top() interface{} {
	if this.IsEmpty() {
		return nil
	}
	return this.topNode.val
}
```

### 2.7、 清除栈的元素

```golang
//清空链表栈
func (this *LinkedListStack) Flush() {
	this.topNode = nil
}
```

### 2.8、 打印出栈的内容

```golang
//打印
func (this *LinkedListStack) Print() {
	if this.IsEmpty() {
		fmt.Println("stack is empty")
	} else {
		cur := this.topNode
		for nil != cur {
			fmt.Println(cur.val)
			cur = cur.next
		}
	}
}
```

## 3、 测试源码

测试方法我上面都追加有测试的命令.可以测试使用

```golang
//测试链表栈
//go test -v -run TestNewLinkedListStack -o StackBaseOnLinkedList_test.go
func TestNewLinkedListStack(t *testing.T) {
	s := NewLinkedListStack()
	s.Print()
}

//测试空链表栈
//go test -v -run TestLinkedListStack_IsEmpty -o StackBaseOnLinkedList_test.go
func TestLinkedListStack_IsEmpty(t *testing.T) {
	s := NewLinkedListStack()
	t.Log(s.IsEmpty())
}

//测试push链表栈
//go test -v -run TestLinkedListStack_Push -o StackBaseOnLinkedList_test.go
func TestLinkedListStack_Push(t *testing.T) {
	s := NewLinkedListStack()
	s.Push(1111)
	s.Push(222)
	s.Print()
}

//测试Pop链表栈
//go test -v -run TestLinkedListStack_Pop -o StackBaseOnLinkedList_test.go
func TestLinkedListStack_Pop(t *testing.T) {
	s := NewLinkedListStack()
	s.Push(1111)
	s.Push(222)
	s.Print()
	t.Log(s.Pop())
	s.Print()
}

//测试Top链表栈
//go test -v -run TestLinkedListStack_Top -o StackBaseOnLinkedList_test.go
func TestLinkedListStack_Top(t *testing.T) {
	s := NewLinkedListStack()
	s.Push(1111)
	s.Push(222)
	s.Print()
	t.Log(s.Top())
}

//测试Top链表栈
//go test -v -run TestLinkedListStack_Flush -o StackBaseOnLinkedList_test.go
func TestLinkedListStack_Flush(t *testing.T) {
	s := NewLinkedListStack()
	s.Push(1111)
	s.Push(222)
	s.Print()
	s.Flush()
	s.Print()
	t.Log(s.IsEmpty())
}
```


## 4、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/StackBaseOnLinkedList.go "基本栈")

* [点击查看测试源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/StackBaseOnLinkedList_test.go "基本栈test")

* [喜欢可以star下🏠](https://github.com/selfjt/algorithm "star")
