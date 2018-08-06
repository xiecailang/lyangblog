---
layout:     post
title:      "盛水最多的容器"
subtitle:   " \"LeetCode\""
date:       2018-08-06 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 题目

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.  
**Note**: You may not slant the container and n is at least 2.

# 思路

$$\mathbf{area} = \mathrm{min}(height[i], height[j]) \times \mathrm{width}$$  
首先，容器的面积等于 **最小** 的高度乘以底边，所以，最小高度的木板越高，就越有可能盛更多的水。同时要得到更大容量的容器，底边也要尽可能长。于是我们可以从最长底边开始，就是 **两端往中间搜索**，搜索过程中 **更新较小高度的一端**，一直到两个搜索指针相遇。

# 代码

```python
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        if len(height) < 2:
            return 0
        if len(height) == 2:
            return min(height[0], height[1])
        max_area = 0
        p_start = 0
        p_end = len(height) - 1

        while p_start < p_end:
            area = (p_end - p_start) * min(height[p_end], height[p_start])
            max_area = max(max_area, area)

            if height[p_start] < height[p_end]:
                p_start += 1
            else:
                p_end -= 1
        return max_area
solution = Solution()
print(solution.maxArea([2,3,4,5,18,17,6]))
```