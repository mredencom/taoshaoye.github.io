---
layout: post
title: 盛最多水的容器算法
categories: Algorithm
tags: [Algorithm]
---
* content
{:toc}

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。


## 场景 

### 中文描述

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
* 你不能倾斜容器，且 n 的值至少为 2。

![max-area](https://www.goroutine.me/assets/images/max-area.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

### English

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
*  You may not slant the container and n is at least 2.

![max-area](https://www.goroutine.me/assets/images/max-area.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

### Example

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```
## 代码示例

```golang
//找出最大的面积区域
func maxArea(height []int) int {
	var max, area int
	start := 0
	end := len(height) - 1 //取数组的长度
	//建立循环取出最大的赋值给max 最后返回max
	for start < end {
		temp := end - start
		if height[start] > height[end] {
			area = temp * height[end]
			end--
		} else {
			area = temp * height[start]
			start++
		}
		if area > max {
			max = area
		}
	}
	return max
}
```
```golang
func main() {
	arr := []int{1, 8, 6, 2, 5, 4, 8, 3, 7}
	c := maxArea(arr)
	fmt.Println(c)
}
```

### 性能

`执行用时 : 36 ms 内存消耗 : 5.7 MB`

## 记录自己学习算法.

* 大家有什么更好解题可以留言.

### 源码

* [点击查看源码](https://github.com/selfjt/algorithm/blob/master/golang/maxArea.go)