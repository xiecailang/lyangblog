---
layout:     post
title:      "Leetcode-最长不重复字串"
subtitle:   " \"刷题\""
date:       2018-07-09 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 题目

Given a string, find the length of the longest substring without repeating characters.
Examples:
Given "abcabcbb", the answer is "abc", which the length is 3.
Given "bbbbb", the answer is "b", with the length of 1.
Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


# 思路

用pre_index记录字串的开始位置，max_记录最长的字串长度，用字典（hash表）dic 记录字串中字符出现的位置。如果当前字符出现在dic中，且记录的位置比pre靠后，则说明字串中有字符与当前字符重复，pre重新赋值为重复字符后一个字符的位置，同时保证pre不要超过当前字符位置。

# 代码

```Python
def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if(len(s) == 0):
            return 0
        if(len(s) == 1):
            return 1
        dic = {}
        dic[s[0]] = 0
        pre_index = 0
        max_ = 1
        for cur_index in range(1, len(s)):
            if s[cur_index] in dic and dic[s[cur_index]] >= pre_index:
                pre_index = min(cur_index, dic[s[cur_index]] + 1)                          
            dic[s[cur_index]] = cur_index                
            max_ = max(cur_index - pre_index + 1, max_)

        return max_
```
