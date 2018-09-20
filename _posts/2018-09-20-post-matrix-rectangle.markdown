---
layout:     post
title:      "最大矩形"
subtitle:   " \"Leetcode\""
date:       2018-09-20 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。  
**示例:**  

    输入:
    [
    ["1","0","1","0","0"],
    ["1","0","1","1","1"],
    ["1","1","1","1","1"],
    ["1","0","0","1","0"]
    ]
    输出: 6

# 解题思路

本题标签包括了动态规划，第一时间考虑用dp算法，但是递归式很难直接给出。于是换了一种思路，求当前行最大面积，与另外一题有密切关系，题目如下：  

    给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。  
    求在该柱状图中，能够勾勒出来的矩形的最大面积。  

![img][img1]

算法：  
输入：矩阵$$matix$$  
输出：最大矩形面积

1. 首先根据给出的matrix将每一行的高度height计算出来  
<center>$$height[i][j] = \left\{ \begin{array}{ll} matrix[i][j] & \textrm{if $i = 0$} \\ height[i-1][j] + 1 & \textrm{if $matix[i][j] = 1$} \\ 0 & \textrm{if $matrix[i][j] = 0$} \end{array} \right.$$</center>  

2. 根据每一行的高度，利用堆栈计算当前行最大的矩形面积
3. 取所有行最大面积的最大值

**堆栈**求一行中最大的矩形的核心思想：最大面积要么高度较高的组合，要么在宽度较宽的组合

堆栈求一行的面积  
假设：$$S_v$$表示垂直方向面积最大值，$$S_h$$表示水平方向面积最大值    
输入：一行高度$$data$$

1. 遍历$$data$$, $$i = 0$$时，建立堆栈$$s$$，将$$data[0]$$入栈，$$S_v = 0$$
2. 比较$$data[i]$$与$$s$$栈顶元素$$top$$
    1. 如果$$data[i] >= top$$，直接入栈
    2. 否则，$$top$$出栈，计数$$cnt = 1, S_v = \max (S_v, S_v*cnt)$$,  将$$data[i]$$继续与栈顶元素比较，重复此步骤，直到栈空或者$$$data[i] >= top$$
3. 计算$$S_h = \max (s[i] * (len - 1))$$，其中$$len$$是堆栈$$s$$的大小
4. 返回$$\max (S_v, S_h)$$

# 代码

```python
class Solution:
    def maxSingleRow(self, data):
        s = []
        res = 0
        for i in range(len(data)):
            d = data[i]
            if i == 0:
                s.append(d)
            else:
                if d >= s[-1]:
                    s.append(d)
                else:
                    cnt = 1
                    while len(s) > 0:
                        top = s.pop()
                        res = max(res, cnt * top)
                        cnt += 1
                        if len(s) == 0 or d >= s[-1]:
                            s.extend([d] * cnt)
                            break
        max_h = max([s[i] * (len(s) - i) for i in range(len(s))])
        return max(res, max_h)
        

    def maximalRectangle(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        m = len(matrix)
        if m == 0:
            return 0
        n = len(matrix[0])
        if n == 0:
            return 0
        height = [[0 for i in range(n)] for j in range(m)]
        for row in range(m):
            for col in range(n):
                if row == 0:
                    height[row][col] = int(matrix[row][col])
                else:
                    height[row][col] = (0 if matrix[row][col] == '0' else height[row - 1][col] + 1)
        s = [self.maxSingleRow(d) for d in height]
        return max(s)

matrix = [
    ["0","0","1","0"],
    ["1","1","1","1"],
    ["1","1","1","1"],
    ["1","1","1","0"],
    ["1","1","0","0"],
    ["1","1","1","1"],
    ["1","1","1","0"]]
so = Solution()
print(so.maximalRectangle(matrix))
```

[img1]:data:image/png;base64,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