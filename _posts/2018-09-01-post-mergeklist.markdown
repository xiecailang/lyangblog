---
layout:     post
title:      "合并K个排序链表"
subtitle:   " \"Leetcode\""
date:       2018-09-01 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# Problem

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。  
示例:  

    输入:
    [
    1->4->5,
    1->3->4,
    2->6
    ]
    输出: 1->1->2->3->4->4->5->6

# 思路

首先可以写出两个链表合并，K个链表最终也要划分到2个链表的合并，比如链表1，2，3，4，5，6，可以1，2合并，合并后再跟3合并，以此类推，但是这种方式复杂度太高，每合并一个，都要遍历合并后的元素，而随着合并进行，合并后的链表的长度一直在增加。  
可以用分治方法来优化，即1，4合并到1，2，5合并到2，3，6合并到3，然后1跟3合并到1，最后1跟2合并到1

# 代码

```python
#合并k个链表
class ListNode:
     def __init__(self, x):
         self.val = x
         self.next = None

class Solution:
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if l1 == None:
            return l2
        if l2 == None:
            return l1
        p1 = l1
        p2 = l2
        h3 = ListNode(0)
        p3 = h3
        while p1 != None and p2 != None:
            t3 = ListNode(min(p1.val,p2.val))
            if p1.val <= p2.val:
                p1 = p1.next
            else:
                p2 = p2.next
            p3.next = t3
            p3 = p3.next
        if p1 == None and p2 != None:
            p3.next = p2
        elif p2 == None and p1 != None:
            p3.next = p1
        return h3.next

    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        if len(lists) == 0:
            return None
        if len(lists) == 1:
            return lists[0]
        if len(lists) == 2:
            return self.mergeTwoLists(lists[0], lists[1])
        n = len(lists)
        while n > 1:
            k = int((n+1)/2)
            for i in range(0, int(n/2)):
                lists[i] = self.mergeTwoLists(lists[i], lists[i+k])
            n = k
        return lists[0]

n1 = [int(i) for i in input().split()]
n2 = [int(i) for i in input().split()]
n3 = [int(i) for i in input().split()]

l1 = ListNode(0)
l2 = ListNode(0)
l3 = ListNode(0)

p1 = l1
p2 = l2
p3 = l3

for i in n1:
    t = ListNode(i)
    p1.next = t
    p1 = t

for i in n2:
    t = ListNode(i)
    p2.next = t
    p2 = t

for i in n3:
    t = ListNode(i)
    p3.next = t
    p3 = t

lists = [l1, l2, l3]
so = Solution()
ans = so.mergeKLists(lists)

p = ans
while p != None:
    print(p.val, end = '->')
    p = p.next
```