---
layout:     post
title:      "翻转链表"
subtitle:   " \"Leetcode\""
date:       2018-08-02 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

> 非递归与递归

# 非递归方式

这种方式思想很简单，从前往后，用两个指针new_head和p分别来保存新链表的头指针和剩下链表的头指针，如：  
1->2->3->4->null  
1. new_head->1->2->3->4->null, p->1->2->3->4->null  
2. new_head->1->null, p->2->3->4->null  
3. new_head->1->2->null, p->3->4->null  
4. ...

# 递归方式

刚好相反，递归是从后往前开始的，先定位到最后一个节点，翻转，结合代码看更清楚

```cpp
node* In_reverseList(node* H)
{
    if (H == NULL || H->next == NULL)       //链表为空直接返回，而H->next为空是递归基
        return H;
    node* newHead = In_reverseList(H->next); //一直循环到链尾
    H->next->next = H;                       //翻转链表的指向
    H->next = NULL;                          //记得赋值NULL，防止链表错乱
    return newHead;                          //新链表头永远指向的是原链表的链尾
}
```