---
layout:     post
title:      "LeetCode | Single Number II"
subtitle:   "Given an array of integers, every element appears three times except for one. Find that single one."
date:       2014-07-14 21:15
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/single-number-ii/)
> Given an array of integers, every element appears three times except for one. Find that single one.

### 解题思路
  * 方法1  
  通过一个HashMap来统计数字出现的次数，如果数字出现次数达到3，则将其移出HashMap，那么最终剩下的即为只出现过一次的那个数字。
  * 方法2  
  将数字表示为32位二进制数的形式，那么每个数字就等价于：32位二进制中，某些位的值为1。将这些数字按位相加，每一位统计1出现的次数并对3取余，那么最终的结果即为只出现过一次的那个数字。  

  方法1和方法2都只需要O(N)的时间复杂度，但是方法1的空间复杂度为O(N)，方法2的空间复杂度虽然为O(1)，但是需要开辟一个大小32的整型数组。那么，不需要额外的空间，是否可以实现呢？
  答案是肯定的：参考[http://www.cnblogs.com/daijinqiao/p/3352893.html](http://www.cnblogs.com/daijinqiao/p/3352893.html)

### 实现代码
```
/**
 * LeetCode | Single Number
 * @author wzystal
 */
public class Solution {
	// Given an array of integers, every element appears three times except for one. Find that single one.
	// 时间复杂度O(N),空间复杂度O(N)
	public static int singleNumber1(int[] A) {
		Map<Integer, Integer> map = new HashMap<Integer, Integer>();
		int len = A.length;
		for (int i = 0; i < len; i++) {
			if (map.containsKey(A[i])) {
				int count = map.get(A[i]);
				if (count == 2) {
					map.remove(A[i]);
				} else {
					map.put(A[i], ++count);
				}
			} else {
				map.put(A[i], 1);
			}
		}
		return map.keySet().iterator().next();
	}
	
	// Given an array of integers, every element appears three times except for one. Find that single one.
	// 时间复杂度O(N),空间复杂度O(1),但是需要开辟一个大小为32的整型数组
	public static int singleNumber2(int[] A) {
		if (A.length == 0)
			return 0;
		int[] bits = new int[32];
		int result = 0;
		for (int i = 0; i < 32; i++) {
			for (int j = 0; j < A.length; j++) {
				if (((A[j] >> i) & 1) == 1) {
					bits[i] = (bits[i] + 1) % 3;
				}
			}
			result |= (bits[i] << i);
		}
		return result;
	}
	
	// Given an array of integers, every element appears three times except for one. Find that single one.
	// 时间复杂度O(N),空间复杂度O(1)
	public static int singleNumber3(int[] A) {
		int len = A.length;
		if (len == 0)
			return 0;
		int ones = 0, twos = 0, threes = 0;
		for (int i = 0; i < len; i++) {
			twos |= (ones & A[i]);
			ones ^= A[i];
			threes = ~(ones & twos);
			ones &= threes;
			twos &= threes;
		}
		return ones;
	}
}
```


