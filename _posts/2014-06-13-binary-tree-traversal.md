---
layout:     post
title:      "LeetCode | Binary Tree traversal"
subtitle:   "分别使用递归、非递归方法，实现二叉树的先序、中序和后序遍历。"
date:       2014-06-13 09:02
author:     "wzystal"
header-img: "img/post-bg-leetcode.jpg"
tags:
    - 算法
    - LeetCode
---

### 原题描述
> 分别使用递归、非递归方法，实现二叉树的先序、中序和后序遍历。

### 解题思路
使用递归方法实现二叉树的遍历，简单至极，此处不再赘述。
对于非递归方法，可以采用栈这种“先进后出”的数据结构，然后根据不同遍历方法的特点，画图分析其入栈、出栈的时机，最后将其转化成代码实现。三种遍历方法的实现流程如下：

##### 先序遍历  
> 1. 根结点入栈；
2. 对于任一栈顶结点P，访问P并将其出栈；
3. 若P有右结点，则将P的右结点入栈；若P有左结点，则将P的左结点入栈；
4. 循环跳转到步骤2，直到栈为空。  

##### 中序遍历  
>1. 当前结点初始化为根结点；
2. 对于任一当前结点P：若P有左结点，则将P入栈，并将当前结点置为P的左结点；否则访问栈顶结点将其出栈，并将当前结点置为P的右结点；
3. 循环跳转到步骤2，直到栈为空或者当前结点为空。  
        
##### 后序遍历  
后序遍历的关键点，是要保证根结点必须要其孩子结点之后被访问，因此当一个结点要出栈时，该结点要么是叶子结点，要么前一个出栈的结点为该结点的孩子结点。这就需要额外维护一个结点pre，用来记录前一个出栈的结点。具体流程如下：
>1. 根结点入栈；
2. 对于任一栈顶结点P，若P为叶子结点或者前一个出栈结点为该结点的孩子结点，则访问P并将其出栈，同时将P设置为前一个结点pre，供下次要出栈的结点参考（比较pre是否为下次出栈结点的孩子结点）；否则，依次将P的右结点和左结点入栈。
3. 循环跳转到步骤2，直到栈为空。

### 实现代码
```
public class Solution {  
    class TreeNode {  
        int val;  
        TreeNode left;  
        TreeNode right;  
  
        public TreeNode(int x) {  
            val = x;  
        }  
    }  
  
    // 先序遍历--递归实现  
    public List<Integer> preorderTraversal(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null) {  
            return list;  
        }  
        list.add(root.val);  
        list.addAll(preorderTraversal(root.left));  
        list.addAll(preorderTraversal(root.right));  
        return list;  
    }  
  
    // 中序遍历--递归实现  
    public List<Integer> inorderTraversal(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null) {  
            return list;  
        }  
        list.addAll(inorderTraversal(root.left));  
        list.add(root.val);  
        list.addAll(inorderTraversal(root.right));  
        return list;  
    }  
  
    // 后序遍历--递归实现  
    public List<Integer> postorderTraversal(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null) {  
            return list;  
        }  
        list.addAll(postorderTraversal(root.left));  
        list.addAll(postorderTraversal(root.right));  
        list.add(root.val);  
        return list;  
    }  
  
    // 先序遍历--非递归实现  
    public List<Integer> preorderStack(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null)  
            return list;  
        Stack<TreeNode> stack = new Stack<TreeNode>();  
        stack.push(root);  
        TreeNode cur = null;  
        while (!stack.empty()) {  
            cur = stack.pop();  
            list.add(cur.val);  
            //先压入右结点，在压入左结点--栈的"先进后出"特性  
            if (cur.right != null)   
                stack.push(cur.right);  
            if (cur.left != null)  
                stack.push(cur.left);  
        }  
        return list;  
    }  
  
    // 中序遍历--非递归实现  
    public List<Integer> inorderStack(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null)  
            return list;  
        Stack<TreeNode> stack = new Stack<TreeNode>();  
        TreeNode cur = root;  
        while (!stack.empty() || cur != null) {  
            if (cur != null) {  
                stack.push(cur);  
                cur = cur.left;  
            } else {  
                TreeNode temp = stack.pop();  
                list.add(temp.val);  
                cur = temp.right;  
            }  
        }  
        return list;  
    }  
  
    // 后序遍历--非递归实现  
    public List<Integer> postorderStack(TreeNode root) {  
        ArrayList<Integer> list = new ArrayList<Integer>();  
        if (root == null)  
            return list;  
        Stack<TreeNode> stack = new Stack<TreeNode>();  
        stack.push(root);  
        TreeNode pre = null;  
        while (!stack.empty()) {  
            TreeNode cur = stack.peek();  
            if ((cur.left == null && cur.right == null) ||   
                (pre != null && (pre == cur.left || pre == cur.right))) {  
                list.add(cur.val);  
                stack.pop();  
                pre = cur;  
            }else {  
                if (cur.right != null)  
                    stack.push(cur.right);  
                if (cur.left != null)  
                    stack.push(cur.left);  
            }  
        }  
        return list;  
    }  
}  
```


