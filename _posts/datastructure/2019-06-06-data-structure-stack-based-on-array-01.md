---
layout: post
title: 数据结构-栈操作-用数组实现栈基本操作
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

### 2.1、 基于数组使用栈的操作

```golang
基于数组使用栈的操作
type ArrayStack struct {
	//数据源
	data []interface{}
	//栈顶标识(指针)
	top int
}
```
### 2.2、 创建一个栈

```golang
//创建一个栈 基于同一个栈操作 需要使用指针
func NewArrayStack() *ArrayStack {
	return &ArrayStack{
		data: make([]interface{}, 0, 32),
		top:  -1,
	}
}
```

### 2.3、 判断栈是否是一个空栈

```golang
//根据栈顶指针可以判断这个栈是否为空栈 空返回true
func (this *ArrayStack) IsEmpty() bool {
	if this.top < 0 {
		return true
	}
	return false
}
```

### 2.4、 向栈添加元素

```golang
//向栈添加元素
func (this *ArrayStack) Push(v interface{}) {
	//增加栈的指针位置
	if this.top < 0 {
		this.top = 0
	} else {
		//这个是在原有的基础上增加一个元素
		this.top += 1
	}
	if this.top > len(this.data)-1 {
		//当栈顶元素长度长于定义数组长度 需要使用slice向后增加 增加定义栈的内存空间
		this.data = append(this.data, v)
	} else {
		//向栈添加到指定指针位置的元素
		this.data[this.top] = v
	}
}
```

### 2.5、 删除栈元素

```golang
//删除栈顶的元素 并返回其元素
func (this *ArrayStack) Pop() interface{} {
	if this.IsEmpty() {
		return nil
	}
	v := this.data[this.top]
	this.top -= 1
	return v
}
```

### 2.6、 取的栈顶的元素

```golang
//取的栈顶的元素
func (this *ArrayStack) Top() interface{} {
	if this.IsEmpty() {
		return nil
	}
	return this.data[this.top]
}
```

### 2.7、 清除栈的元素

```golang
//清除栈的元素
func (this *ArrayStack) Flush() {
	this.top = -1
}
```

### 2.8、 打印出栈的内容

```golang
//打印出栈的内容
func (this *ArrayStack) Print() {
	if this.IsEmpty() {
		fmt.Println("stack is empty")
	} else {
		for i := this.top; i >= 0; i-- {
			fmt.Println(this.data[i])
		}
	}
}
```

## 3、 测试源码

测试方法我上面都追加有测试的命令.可以测试使用

```golang
//测试新建栈
//go test -v -run TestNewArrayStack -o StackBaseOnArray_test.go
func TestNewArrayStack(t *testing.T) {
	//新建一个栈
	s := NewArrayStack()
	//打印栈
	s.Print()
}

//测试向栈加入元素
//go test -v -run TestArrayStack_Push -o StackBaseOnArray_test.go
func TestArrayStack_Push(t *testing.T) {
	//新建一个栈
	s := NewArrayStack()
	//向栈加入元素
	s.Push(11111)
	s.Push("你好")
	s.Push("hello stack")
	s.Print()
	//取栈顶元素
	t.Log(s.top)
}

//测试栈是否为空
//go test -v -run TestArrayStack_IsEmpty -o StackBaseOnArray_test.go
func TestArrayStack_IsEmpty(t *testing.T) {
	s := NewArrayStack()
	t.Log("空栈: ", s.IsEmpty())
	s.Push(111)
	t.Log("非空栈: ", s.IsEmpty())
}

//测试删除栈顶元素 并返回
//go test -v -run TestArrayStack_Pop -o StackBaseOnArray_test.go
func TestArrayStack_Pop(t *testing.T) {
	//新建一个栈
	s := NewArrayStack()
	s.Push(11111)
	s.Push("你好")
	s.Push("需要删除的栈元素")
	s.Print()
	d := s.Pop()
	t.Log("需要删除的栈元素: ", d)
	t.Log("------------------------")
	s.Print()
}

//测试取栈顶元素
//go test -v -run TestArrayStack_Top -o StackBaseOnArray_test.go
func TestArrayStack_Top(t *testing.T) {
	//新建一个栈
	s := NewArrayStack()
	s.Push(11111)
	s.Push("你好")
	s.Push("待取的栈顶元素")
	s.Print()
	t.Log(s.Top())
}

//测试取栈顶元素
//go test -v -run TestArrayStack_Flush -o StackBaseOnArray_test.go
func TestArrayStack_Flush(t *testing.T) {
	//新建一个栈
	s := NewArrayStack()
	s.Push(11111)
	s.Push("你好")
	s.Print()
	s.Flush()
	s.Print()
}
```


## 4、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/StackBaseOnArray.go "基本栈")

* [点击查看测试源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/StackBaseOnArray_test.go "基本栈test")

* [喜欢可以star下🏠](https://github.com/selfjt/algorithm "star")
