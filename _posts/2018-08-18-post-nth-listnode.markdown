---
layout:     post
title:      "删除链表倒数第N个元素"
subtitle:   " \"Leetcode\""
date:       2018-08-18 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# Problem

Given a linked list, remove the n-th node from the end of list and return its head.

    Example:
    Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.  
**Note**:  
Given n will always be valid.  
**Follow up**:  
Could you do this in one pass?  

# 思路

双指针，两个指针相差n，向后遍历，当后一个指针到达end，则pre指针刚好在倒数n

# code

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        if n == 1 and head.next == None:
            return None
        p_end = head
        p_n = ListNode(-1)
        p_n.next = head
        cnt = 0
        len_ = 0
        while p_end != None:  
            len_ += 1          
            if cnt < n:
                cnt += 1
            else:
                p_n = p_n.next            
            p_end = p_end.next
        if len_ == n:
            head = head.next
        else:
            p_n.next = p_n.next.next
                           
        return head

nums = [int(i) for i in input().split()]
n = int(input())
head = ListNode(nums[0])
p = head
for i in range(1, len(nums)):
    temp = ListNode(nums[i])
    p.next = temp
    p = temp


sol = Solution()
head = sol.removeNthFromEnd(head, n)

p = head
while p != None:
    print('%d->' % p.val, end = '')
    p = p.next
print('None')
```