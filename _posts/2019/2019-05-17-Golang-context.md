---
layout: post
title: 初识golang中Context
category: golang
tags: [golang,context]
---

在刚刚过去的 2019 gopher china 大会上 context 概念被多次提起，包括很多框架的源码也大量运用了。看得出来 context 在 golang的世界中是一个非常重要的知识点，所以有必要对 context 有一个基本的使用和认知。官方文档解释和示例都比较详细正规，本着学习的态度翻译一遍加深理解。


## 一、概述

context 包定义了 Context 类型，它在 API 边界和进程之间传递截止时间，取消信号和其他请求作用域的值。服务收到请求应该要创建一个 Context，对服务的响应应该要接受一个 Context。它们之间的函数调用链必须传递 Context，也可以使用 WithCancel，WithDeadline，WithTimeout 或 WithValue 等方法创建派生 Context 替换它。取消 Context 后，也会取消从中派生的所有 Context。
WithCancel，WithDeadline 和 WithTimeout 函数接受 Context（父）并返回派生的 Context（子）和一个 CancelFunc 函数。调用 CancelFunc 函数会取消该派生的子 Context 及其孙子 Context，删除父项对子项的引用，并停止任何关联的计时器。如果没有调用 CancelFunc 会泄漏子项和孙子项，直到父项被取消或计时器触发。 go vet 工具检查是否在所有控制流路径上使用了 CancelFuncs。
使用 Contexts 的程序应遵循这些规则，以保持各个包的接口一致，并启用静态分析工具来检查上下文的传递：
不要将 Contexts 存储在结构类型中；相反，要将 Context 明确地传递给需要它的每个函数。 Context 应该是第一个参数，通常命名为 ctx：

{% highlight golang %}

func DoSomething(ctx context.Context, arg Arg) error {
	// ... use ctx ...
}

{% endhighlight %}

即使函数允许，也不要传递 nil Context。如果你不确定要使用哪个 Context，请传递 context.TODO。
仅将上下文的值用于 API 边界和进程之间的请求作用域数据，而不是将可选参数传递给函数。
可以将相同的 Context 传递给在不同 goroutine 中运行的函数；Contexts 对于同时使用多个 goroutine 是安全的。
有关服务中使用 Contexts 的示例代码，请参考[blog.golang.org/context.](https://blog.golang.org/context)

## 二、变量
Canceled 是上下文取消时，通过 Context.Err 返回的错误。
```
	var Canceled = errors.New("context canceled")
```
DeadlineExceeded 是在上下文超过截止时间时，通过 Context.Err 返回的错误。
```
	var DeadlineExceeded error = deadlineExceededError{}
```

## 参考

* [Run in playground]()

	