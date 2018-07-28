---
layout:     post
title:      "判断字符串是否是数字"
subtitle:   " \"Leetcode\""
date:       2018-07-28 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# problem

Validate if a given string is numeric.
Some examples:
    "0" => true
    " 0.1 " => true
    "abc" => false
    "1 a" => false
    "2e10" => true
Note: It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.

# 思路

有两个解法，一个是正则表达式，一个是try float()

正则表达式符号说明
* '^' 表示字符串开始
* '?' 0个或1个
* '+' 至少一个
* '*' 任意个
* '\s' 空格
* '$' 字符串结束
* '|' 或

# Code

```python
class Solution:
    def isNumber2(self, s):
        try:
            float(s)
            return True
        except:
            return False
    def isNumber(self, s):
        """ 
        :type s: str
        :rtype: bool
        """
        import re
        value = re.compile(r'^\s*[-+]?[0-9]+\.?[0-9]*\s*$|^\s*[-+]?[0-9]*\.?[0-9]+\s*$|^\s*[-+]?[0-9]+\.?[0-9]*e[-+]?[0-9]+\s*$|^\s*[-+]?[0-9]*\.?[0-9]+e[-+]?[0-9]+\s*$')

        res = value.match(s)
        if res:
            return True
        else:
            return False

solution = Solution()
print(solution.isNumber2('sn'))
```
 

