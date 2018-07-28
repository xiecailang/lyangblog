---
layout:     post
title:      "string 转 int"
subtitle:   " \"Leetcode\""
date:       2018-07-28 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# problem

Implement atoi which converts a string to an integer.
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
If no valid conversion could be performed, a zero value is returned.

* Note
    * Only the space character ' ' is considered as whitespace character.
    * Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $$[−2^{31},  2^{31} − 1]$$. If the numerical value is out of the range of representable values, INT_MAX $$(2^{31} − 1)$$ or INT_MIN $$(−2^{31})$$ is returned.

# 思路

这题目看似不难，实则有很多坑，如下

* 前面的字符有空格，又有‘-’或者‘+’
    Input: '   -42'
    Output: 42

* 中间的字符有空格
    Input: '   +0 123'
    Output: 0

* 前面的字符有多个正负符号
    Input: '+-2'
    Output: 0

# Code

```python
class Solution:
    def myAtoi2(self, str):
        first_char = False
        sign = 1
        res = 0
        for c in str:
            if c == ' ' and not first_char:
                continue
            elif c == '-' and not first_char:
                sign = -1
                first_char = True
            elif c == '+' and not first_char:
                first_char = True
            elif ord(c) >= ord('0') and ord(c) <= ord('9'):
                first_char = True
                res = res * 10 + ord(c) - ord('0')
            else:
                break
        res *= sign
        if res < -2**31:
            res = -2**31
        if res > 2**31 - 1:
            res = 2**31 - 1
        return res
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        res = 0
        sign = 1
        first_char = False
        if len(str) == 0:
            return 0
        else:
            i = 0
            while i < len(str):
                c = str[i]
                i += 1
                if not first_char:
                    if c == ' ':
                        continue
                    elif c == '-' or c == '+':
                        sign = -1 if c == '-' else 1
                        first_char = True
                        continue
                    else:
                        first_char = True
                        i = 0 if i - 1 < 0 else i - 1
                        continue
                else:
                    if ord(c) < ord('0') or ord(c) > ord('9'):
                        break
                    res = res * 10 + ord(c) - ord('0')
                
        res *= sign
        if res < -2**31:
            res = -2**31
        if res > 2**31 - 1:
            res = 2**31 - 1
        return res

solution = Solution()
print(solution.myAtoi2('42'))
```
 

