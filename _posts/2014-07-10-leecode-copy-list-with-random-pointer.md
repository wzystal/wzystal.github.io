---
layout:     post
title:      "LeetCode | Copy List with Random Pointer"
subtitle:   "A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null. Return a deep copy of the list."
date:       2014-07-14 19:21
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/copy-list-with-random-pointer/)
> A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
Return a deep copy of the list.

### 解题思路
本题要复制的对象是带随机指针的单链表，随机指针指向单链表的某一个结点。
参考文章：[http://blog.csdn.net/linhuanmars/article/details/22463599](http://blog.csdn.net/linhuanmars/article/details/22463599)

### 实现代码
```
/** 
 * Copy List with Random Pointer 
 * @author wzystal 
 */  
public class Solution {  
    class RandomListNode {  
        int label;  
        RandomListNode next, random;  
  
        public RandomListNode(int x) {  
            this.label = x;  
        }  
    }  
  
    // 时间复杂度O(n),空间复杂度O(n)  
    public RandomListNode copyRandomList1(RandomListNode head) {  
        if (head == null)  
            return head;  
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>();  
        RandomListNode newHead = new RandomListNode(head.label);  
        map.put(head, newHead);  
        RandomListNode pre = newHead;  
        RandomListNode node = head.next;  
        while (node != null) {  
            RandomListNode newNode = new RandomListNode(node.label);  
            map.put(node, newNode);  
            pre.next = newNode;  
            pre = pre.next;  
            node = node.next;  
        }  
        node = head;  
        RandomListNode copyNode = newHead;  
        while (node != null) {  
            copyNode.random = map.get(node.random);  
            copyNode = copyNode.next;  
            node = node.next;  
        }  
        return newHead;  
    }  
  
    // 时间复杂度O(n),空间复杂度O(1)  
    public RandomListNode copyRandomList2(RandomListNode head) {  
        if (head == null)  
            return head;  
        RandomListNode node = head;  
        while (node != null) {  
            RandomListNode newNode = new RandomListNode(node.label);  
            newNode.next = node.next;  
            node.next = newNode;  
            node = newNode.next;  
        }  
        node = head;  
        while (node != null) {  
            if (node.random != null) {  
                node.next.random = node.random.next;  
            }  
            node = node.next.next;  
        }  
        RandomListNode newHead = head.next;  
        node = head;  
        while (node != null) {  
            RandomListNode newNode = node.next;  
            node.next = newNode.next;  
            if (newNode.next != null) {  
                newNode.next = newNode.next.next;  
            }  
            node = node.next;  
        }  
        return newHead;  
    }  
}  
```


