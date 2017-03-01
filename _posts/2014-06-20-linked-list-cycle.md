---
layout:     post
title:      "LeetCode | Linked List Cycle"
subtitle:   "判断一个单链表是否有环，若有环的话，返回环的入口点。空间复杂度为O(1)"
description: "LeetCode算法实现 - Linked List Cycle"
date:       2014-06-20 20:50
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/linked-list-cycle/)
> 判断一个单链表是否有环，若有环的话，返回环的入口点。空间复杂度为O(1)。

### 解题思路
快慢指针法。慢指针步长为1，快指针步长为2，两个指针同时从头结点开始扫描，若单链表存在环，则两个指针必定会相遇，而且是在慢指针遍历完环之前。原理解析可以参考：[http://blog.csdn.net/loveyou426/article/details/7927297](http://blog.csdn.net/loveyou426/article/details/7927297)。

那么环的入口点又该怎么找呢？  

我们假设单链表长度为len，头结点到环入口点的距离为a，环入口点到相遇点的距离为b，整个环的长度为r。显然有len=a+r。
>1. 由于相遇时慢指针还未转完一圈，所以慢指针的"行程"为(a+b)，快指针的"行程"则为慢指针"行程"的两倍:2(a+b);
2. 我们假设快慢指针相遇时，快指针已经绕了环n圈，显然快指针的"行程"可以计算为:(a+b)+n*r; 
3. 结合1、2可知: 2(a+b)=(a+b)+n*r,即(a+b)=n*r --> (a+b)=(n-1)*r+r --> (a+b)=(n-1)*r+(len-a) --> a=(n-1)*r+(len-a-b)。其中(len-a-b)正好是相遇点到环入口的距离。也就是说，指针从头结点到环入口的距离，正好等于指针从相遇点到环入口的距离！
4. 由3中结论，我们找到相遇点以后，可以同时设置两个指针，分别从头结点和相遇点开始遍历，由于头结点到环入口的距离等于相遇点到环入口的距离，所以当两个指针相遇时，它们的相遇点即为环的入口点！

### 实现代码
```
/** 
 * LeetCode | Linked List Cycle 
 * @author Young Z. Wang 
 */  
public class Solution {  
    class ListNode {  
        int val;  
        ListNode next;  
  
        public ListNode(int val) {  
            this.val = val;  
            this.next = null;  
        }  
    }  
  
    // 判断单链表是否有环  
    public boolean hasCycle(ListNode head) {  
        if (head == null || head.next == null) return false;  
        ListNode slow = head, fast = head;  
        while (fast != null && fast.next != null) {  
            slow = slow.next;  
            fast = fast.next.next;  
            if (fast == slow) return true;  
        }  
        return false;  
    }  
  
    // 若单链表存在环，返回环的入口点  
    public ListNode detectCycle(ListNode head) {  
        if (head == null || head.next == null) return null;  
        ListNode slow = head, fast = head;  
        while (fast != null && fast.next != null) {  
            slow = slow.next;  
            fast = fast.next.next;  
            if (fast == slow) break;  
        }  
        if (fast == slow) {  
            ListNode n1 = head, n2 = slow;  
            while(n1 != n2){  
                n1 = n1.next;  
                n2 = n2.next;  
            }  
            return n1;  
        }  
        return null;  
    }  
}  
```


