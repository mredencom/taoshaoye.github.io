---
layout: post
title: 数据结构-队列操作-用数组实现队列基本操作
categories: DataStructure
tags: [DataStructure]
---
* content
{:toc}

数据结构中的队列基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取队列的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 1、 场景 

### 1.1、 中文描述

数据结构中的队列基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取队列的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 2、 代码示例

### 2.1、 定义一个队列数据结构

```golang
//定义一个队列数据结构
type ArrayQueue struct {
	q        []interface{} //值
	capacity int           //容量
	head     int           //头节点
	tail     int           //尾部节点
}
```
### 2.2、 新建一个数组队列

```golang
//新建一个数组队列
func NewArrayQueue(n int) *ArrayQueue {
	return &ArrayQueue{make([]interface{}, n), n, 0, 0}
}
```

### 2.3、 检查队列是否还可以追加元素 可以追加就加在队列上 返回状态

```golang
//检查队列是否还可以追加元素 可以追加就加在队列上 返回状态
func (this *ArrayQueue) EnQueue(v interface{}) bool {
	//检查队列长度是否等于tail节点
	if this.capacity == this.tail {
		return false
	}
	this.q[this.tail] = v
	this.tail++
	return true
}
```

### 2.4、 取出头节点数据 并删除

```golang
//取出头节点数据 并删除
func (this *ArrayQueue) DeQueue() interface{} {
	//检查对了是否有数据
	if this.head == this.tail {
		return nil
	}
	v := this.q[this.head]
	//头节点后移
	this.head++
	return v
}
```

### 2.5、 输出队列数据

```golang
//输出队列数据
func (this *ArrayQueue) String() string {
	//检查队列是否有数据
	if this.head == this.tail {
		return "Queue is empty"
	}
	result := "head"
	for i := this.head; i <= this.tail-1; i++ {
		result += fmt.Sprintf("<-%v", this.q[i])
	}
	result += "<-tail"
	return result
}
```

## 3、 测试源码

测试方法我上面都追加有测试的命令.可以测试使用

```golang
//测试EnQueue 方法
//go test -v -run TestArrayQueue_EnQueue -o QueueBaseOnArray_test.go
func TestArrayQueue_EnQueue(t *testing.T) {
	q := NewArrayQueue(10)
	q.EnQueue(1)
	q.EnQueue(2)
	q.EnQueue(3)
	t.Log(q.String())

}

//测试DeQueue 方法
//go test -v -run TestArrayQueue_DeQueue -o QueueBaseOnArray_test.go
func TestArrayQueue_DeQueue(t *testing.T) {
	q := NewArrayQueue(10)
	q.EnQueue(1)
	q.EnQueue(2)
	q.EnQueue(3)
	//清除元素
	t.Log(q.String())
	q.DeQueue()
	t.Log(q.String())

}

```


## 4、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/queue/QueueBaseOnArray.go "基本队列")

* [点击查看测试源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/QueueBaseOnArray_test.go "基本队列test")

* [喜欢可以star下🏠](https://github.com/selfjt/algorithm "star")
