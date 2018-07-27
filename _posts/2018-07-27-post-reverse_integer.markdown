---
layout:     post
title:      "反转Int"
subtitle:   " \"Leetcode\""
date:       2018-07-27 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

 # problem

 Given a 32-bit signed integer, reverse digits of an integer.

    Input: 123
    Output: 321

# 思路

从x的最低位往前取，放到res的最高位，注意区分符号，x和res的范围要在$$[-2^{31}, 2^{31}-1]$$内

# Code

```python
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x < -2**31 or x > 2**31 - 1:
            return 0
        sign = -1 if x < 0 else 1
        temp = sign * x
        res = 0
        while temp > 0:
            i = temp % 10
            res = res * 10 + i
            temp = int(temp/10)
        res *= sign

        if res < -2**31 or res > 2**31 - 1:
            return 0
        return res

solution = Solution()
print(solution.reverse(1534236469))
```
 

