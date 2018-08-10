---
layout:     post
title:      "Letter Combinations of a Phone Number"
subtitle:   " \"Leetcode\""
date:       2018-08-10 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# Problem

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.  
A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.  
nums = {'2':'abc','3':'def','4':'ghi','5':'jkl','6':'mno','7':'pqrs','8':'tuv','9':'wxyz'}  
**Example**  

    Input: "23"
    Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

# 思路

迭代，试想当digits只有两位数字，比如‘23’，那么其组合可以列举出来，如果digits有3位数字，如‘234’，那么只要将列举出来的‘23’再跟‘4’一起组合即可，循环可得解

# 代码

```python
class Solution:
    def listCombinations(self, list_1, list_2):
        ans = []
        if len(list_1) == 0:
            return list_2
        if len(list_2) == 0:
            return list_1
        for c1 in list_1:
            for c2 in list_2:
                ans.append(c1+c2)
        return ans

    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        ans = []
        nums = {'2':'abc','3':'def','4':'ghi','5':'jkl','6':'mno','7':'pqrs','8':'tuv','9':'wxyz'} 
        for n in digits:
            list_1 = list(nums[n])
            ans = self.listCombinations(ans, list_1)
        return ans
solution = Solution()
print(solution.letterCombinations('234'))
```
