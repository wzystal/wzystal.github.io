---
layout:     post
title:      "LeetCode | Single Number"
subtitle:   "Given an array of integers, every element appears twice except for one. Find that single one."
date:       2014-07-14 19:21
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/single-number/)
> Given an array of integers, every element appears twice except for one. Find that single one.

### 解题思路
可以利用异或运算来实现。关于异或运算的几个法则：
> a ^ a = 0;
a ^ 0 = a;
a ^ b = b ^ a ; 

### 实现代码
```
/**
 * LeetCode | Single Number
 * @author wzystal
 */
public class Solution {
	public int singleNumber(int[] A) {
		int result = 0;
		for (int i : A) {
			result ^= i;
		}
		return result;
	}
}
```


