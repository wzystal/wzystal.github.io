---
layout:     post
title:      "LeetCode | Word Break II"
subtitle:   "Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.
Return all such possible sentences."
description: "LeetCode算法实现 - Word Break II"
date:       2014-07-02 19:21
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/word-break-ii/)
> Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given
s = "catsanddog",
dict = ["cat", "cats", "and", "sand", "dog"].

A solution is ["cats and dog", "cat sand dog"].

### 解题思路
动态规划+DFS

### 实现代码
```
/** 
 * LeetCode | Word Break II 
 * @author wzystal@gmail.com 
 */  
public class Solution {  
    List<String> records = new ArrayList<String>();  
  
    public List<String> wordBreak(String s, Set<String> dict) {  
        if (canBreak(s, dict)) {  
            dfsSearch(s, dict, "");  
        }  
        return records;  
    }  
  
    private boolean canBreak(String s, Set<String> dict) {  
        if (s == null || s.length() == 0 || dict == null || dict.size()==0)  
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
  
    private void dfsSearch(String str, Set<String> dict, String result) {  
        int len = str.length();  
        if (len == 0) {  
            records.add(result);  
        }  
        for (int i = 0; i <= len; i++) {  
            String sub = str.substring(0,i);  
            if(dict.contains(sub)){  
                int curLen = result.length();  
                if(!result.equals("")){  
                    result += " ";  
                }  
                result += sub;  
                dfsSearch(str.substring(i), dict, result);  
                result = result.substring(0, curLen);  
            }  
        }  
    }  
}  
```


