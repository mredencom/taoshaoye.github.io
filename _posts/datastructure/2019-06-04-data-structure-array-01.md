---
layout: post
title: 数据结构-数组操作
category: DataStructure
tags: [DataStructure]
---
* content
{:toc}

数据结构中的数组基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取数组的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 1、 场景 

### 1.1、 中文描述

数据结构中的数组基本操作,我这里是也是为了学习记录我自己的书写的代码过程.其中包含取数组的新建,新增元素,删除元素,取指定索引值,向元素尾部追加元素 等等!

## 2、 代码示例

### 2.1、 定义一个数组结构

```golang
//定义一个结构体
type Array struct {
	data   []int //数组
	length int   //长度
}
```
### 2.2、 新建一个数组

```golang
// 我们这里做一个建立数组的方法
func NewArray(capacity uint) *Array {
	//因为我们数组的长度不能为0 因为0初始化没有意义
	if capacity == 0 {
		return nil
	}
	return &Array{
		data:   make([]int, capacity, capacity),
		length: 0,
	}
}
```

### 2.3、 取当前数组的长度

```golang
//取当前数组的长度
func (this *Array) Len() uint {
	return uint(this.length)
}
```

### 2.4、 检查数组是否越界

```golang
//判断数组是否越界  true:越界  false:没有越界 n~n-1
func (this *Array) isIndexOutOfRange(index uint) bool {
	if index >= uint(cap(this.data)) {
		return true
	}
	return false
}

```

### 2.5、 索引查找数组中的数据

```golang
//这个根据索引查找数组中的数据
func (this *Array) Find(index uint) (int, error) {
	if this.isIndexOutOfRange(index) {
		return 0, errors.New("数组下标越界")
	}
	return this.data[index], nil
}
```

### 2.6、 插入指定数组索引值

```golang
//给指定的值插入到指定索引上. 返回错误
func (this *Array) Insert(index uint, v int) error {
	//第一步检查数组的时候已经满载
	if this.Len() == uint(cap(this.data)) {
		return errors.New("数组已经满了!不可增加值")
	}
	if this.isIndexOutOfRange(index) && index != uint(this.length) {
		return errors.New("数组下标越界!")
	}
	for i := this.length; int(index) < i; i-- {
		//index后的元素后移
		this.data[i] = this.data[i-1]
	}
	this.data[index] = v
	this.length++
	return nil
}
```

### 2.7、 追加元素到数组尾部

```golang
//追加元素到数组的尾部
func (this *Array) InsertToTail(v int) error {
	return this.Insert(this.Len(), v)
}
```

### 2.8、 删除指定索引上的值

```golang
//删除指定索引上的值
func (this *Array) Delete(index uint) (int, error) {
	if this.isIndexOutOfRange(index) {
		return 0, errors.New("索引下标越界")
	}
	//去除指定索引上的值
	v := this.data[index]
	//把删除后的元素的其他索引元素前移
	for i := index; i < this.Len()-1; i++ {
		this.data[i] = this.data[i+1]
	}
	//更新数组长度
	this.length--
	return v, nil
}

```

### 2.9、 打印数组

```golang
//输出一个数组
func (this *Array) Print() {
	var format string
	for i := uint(0); i < this.Len(); i++ {
		//拼接数组
		format += fmt.Sprintf("|%+v", this.data[i])
	}
	fmt.Println(format)
}
```

## 3、 测试源码

测试方法我上面都追加有测试的命令.可以测试使用

```golang
//测试数组插入方法 go test -v -run TestArray_Insert -o array_test.go
func TestArray_Insert(t *testing.T) {
	capacity := 10
	arr := NewArray(uint(capacity))
	for i := 0; i < capacity-2; i++ {
		//循环入数组
		err := arr.Insert(uint(i), i+1)
		if err != nil {
			t.Fatal(err.Error())
		}
	}
	arr.Print()
	_ = arr.Insert(uint(6), 999)
	arr.Print()
	_ = arr.InsertToTail(555)
	arr.Print()
}

//测试删除方法 go test -v -run TestArray_Delete -o array_test.go
func TestArray_Delete(t *testing.T) {
	capacity := 10
	arr := NewArray(uint(capacity))
	for i := 0; i < capacity; i++ {
		err := arr.Insert(uint(i), i+1)
		if nil != err {
			t.Fatal(err.Error())
		}
	}
	arr.Print()
	//这里做循环删除
	for i := 9; i >= 0; i-- {
		_, err := arr.Delete(uint(i))
		if nil != err {
			t.Fatal(err)
		}
		arr.Print()
	}
	arr.Print()
}

//测试 go test -v -run TestArray_Find -o array_test.go
func TestArray_Find(t *testing.T) {
	capacity := 10
	arr := NewArray(uint(capacity))
	for i := 0; i < capacity; i++ {
		err := arr.Insert(uint(i), i+1)
		if nil != err {
			t.Fatal(err.Error())
		}
	}
	arr.Print()
	t.Log(arr.Find(0))
	t.Log(arr.Find(9))
	t.Log(arr.Find(11))
}
```


## 4、 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/array/array.go "数组")

* [点击查看测试源码](https://github.com/selfjt/algorithm/blob/master/golang/array/array_test.go "数组test")

* [喜欢可以star下🏠](https://github.com/selfjt/algorithm "star")
