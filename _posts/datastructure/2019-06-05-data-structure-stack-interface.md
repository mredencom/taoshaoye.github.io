---
layout: post
title: 数据结构-栈操作接口定义
category: DataStructure
tags: [DataStructure]
---
数据结构中的栈基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取栈的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 1、 场景 

### 1.1、 中文描述

数据结构中的栈基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取栈的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!
这个接口在后面文章中会使用到.这个为了是统一操作使用

## 2、 代码示例

### 2.1、 定义一个栈操作接口

```golang
//定义栈需要使用的方法
type Stack interface {
	//入栈的方法
	Push(v interface{})
	//删除元素并返回元素
	Pop() interface{}
	//判断栈是否为空
	IsEmpty() bool
	//栈顶元素
	Top() interface{}
	//清空栈-元素
	Flush()
}
```

## 3、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/stack/StackInterface.go "栈操作接口")

* [喜欢可以star下🏠](https://github.com/selfjt/algorithm "star")
