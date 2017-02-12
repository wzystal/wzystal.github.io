---
layout:     post
title:      "LeetCode | Reorder List"
subtitle:   "Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…
You must do this in-place without altering the nodes' values."
date:       2014-06-20 20:17
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### [原题描述](https://oj.leetcode.com/problems/reorder-list/)
> Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…  
You must do this in-place without altering the nodes' values.  
For example,   
Given {1,2,3,4}, reorder it to {1,4,2,3}.

### 解题思路
1. 将单链表从中点处拆分成两部分：head和middle，可以采用快慢指针的方法实现；
2. 将middle部分单链表反转；
3. 穿插式合并head和middle两部分。

### 实现代码
```
//Reorder List  
public class Solution {  
    class ListNode {  
        int val;  
        ListNode next;  
  
        public ListNode(int val) {  
            this.val = val;  
            next = null;  
        }  
    }  
  
    public void reorderList(ListNode head) {  
        if (head == null || head.next == null)  
            return;  
        ListNode fast = head, slow = head;  
        while (fast != null && fast.next != null && fast.next.next != null) {  
            slow = slow.next;  
            fast = fast.next.next;  
        }  
        ListNode middle = slow.next;  
        slow.next = null;  
        middle = reverseList(middle);  
        ListNode n1 = head, n2 = middle;  
        mergeList(n1, n2);  
    }  
  
    //反转单链表  
    private ListNode reverseList(ListNode head) {  
        if (head == null || head.next == null)  
            return head;  
        ListNode pre = head, cur = head.next;  
        while (cur != null) {  
            ListNode temp = cur.next;  
            cur.next = pre;  
            pre = cur;  
            cur = temp;  
        }  
        head.next = null; //important!  
        return pre;  
    }  
  
    //穿插合并单链表  
    private void mergeList(ListNode n1, ListNode n2) {  
        ListNode t1, t2;  
        while (n2 != null) {  
            t1 = n1.next;  
            t2 = n2.next;  
            n1.next = n2;  
            n2.next = t1;  
            n1 = t1;  
            n2 = t2;  
        }  
    }  
}  
```


