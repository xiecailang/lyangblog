---
layout:     post
title:      "搜索旋转排序数组"
subtitle:   " \"Leetcode\""
date:       2018-10-10 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

假设按照升序排序的数组在预先未知的某个点上进行了旋转。  
例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] 。  
搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。  
你可以假设数组中不存在重复的元素。  
你的算法时间复杂度必须是 O(log n) 级别。

    示例 1:
    输入: nums = [4,5,6,7,0,1,2], target = 0
    输出: 4
    示例 2:
    输入: nums = [4,5,6,7,0,1,2], target = 3
    输出: -1

# 思路

参考[LeetCode（33）](https://www.cnblogs.com/ariel-dreamland/p/9138064.html)，复杂度要在 O(log n) 肯定是要用二分查找，与普通排序的二分不一样的是，这里left和right的移动规律不同

1. 当nums[mid] < nums[right]，则mid到right是有序的子数组
2. 当nums[mid] > nums[right], 则left到mid是有序的

再根据nums[mid]和target的大小决定left和right的移动

# 代码

```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] < nums[right]:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
            else:
                if nums[left] <= target <= nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
        return -1

nums = [int(i) for i in input().split()]
target = int(input())
so = Solution()
print(so.search(nums, target))
```