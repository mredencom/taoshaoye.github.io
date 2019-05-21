---
layout: post
title: 两数之和
category: 算法
tags: [算法]
---
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 场景 

### 中文描述

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

### English

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```
## 代码示例

```golang
func twoSum(nums []int, target int) []int {
  //定义一个结果数组
	result := make([]int, 0)
	m := make(map[int]int)
	if nums == nil || len(nums) <= 1 {
		return result
	}
	for i := 0; i < len(nums); i++ {
		num := nums[i]
		r, ok := m[target-num]
		if ok {
			//如果成立写入slice
			result = append(result, r, i)
			return result
		} else {
			m[num] = i
		}
	}
	return result
}
```
## 记录自己学习算法.
* 大家有什么更好的算法可以留言.