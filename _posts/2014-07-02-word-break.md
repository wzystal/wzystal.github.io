---
layout:     post
title:      "LeetCode | Word Break"
subtitle:   "Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words."
date:       2014-07-02 21:28
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/word-break/)
> Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

### 解题思路
动态规划思路求解。
对于任何一个字符串S（长度为N），如果S能被字典集 dict “割裂”（即S能由dict中的单词拼接而成），那么必定有：
>T(0,N) = T(0,i) && T(i,N) ; 
T(i,j)表示字符串S从索引i到j所截取的部分，能被字典集dict割裂。

所以，我们在遍历字符串S的时候，可以基于上述思路，每次将遍历得到的字符串S(0,i)划分为S(0,j)和S(j,i)，然后根据T(0,j) && T(j,i)来判断S(0,i)能否被字典集dict割裂，同时我们在遍历过程中维护一个布尔数组can（can[i]==true表示S(0,i)能被字典集dict割裂），来避免重复计算。这样遍历并判断下去，can[len]的结果就是我们想要的解。

### 实现代码
```
/** 
 * LeetCode | Word Break 
 *  
 * @author wzystal@gmail.com 
 */  
public class Solution {  
    public boolean wordBreak(String s, Set<String> dict) {  
        if (s == null || s.length() == 0 || dict == null || dict.size() == 0)  
            return false;  
        int len = s.length();  
        boolean[] can = new boolean[len + 1];  
        can[0] = true;  
        for (int i = 1; i <= len; i++) {  
            for (int j = 0; j < i; j++) {  
                if (can[j] && dict.contains(s.substring(j, i))) {  
                    can[i] = true;  
                    break;  
                }  
            }  
        }  
        return can[len];  
    }  
}  
```


