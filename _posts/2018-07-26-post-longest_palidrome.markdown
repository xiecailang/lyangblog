---
layout:     post
title:      "LeetCode-最长回文子串"
subtitle:   " \"刷题\""
date:       2018-07-26 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 思路

**Manacher**算法，在字符串之间插入一个额外字符'#'，组成一个新数组new_str,用一个len_数组记录new_str中最长回文子串右边界的长度max_len，记录最长子串中心点位置max_index. 

在new_str中，以i为中心的最长回文子串长度为2len_[i] - 1, 且‘#’的长度总是要比字符数多1，则字符数为len_[i] - 1, '#'的数量为len_[i]

最长回文子串为原字符串[(2 * max_index - (max_index + max_len - 1))/2 : (max_index + max_len - 1)/2]

# 示例

s: "hello"
new_str: "#h#e#l#l#o#"
len_: [1, 2, 1, 2, 1, 2, 3, 2, 1, 2, 1]
max_index: 6
max_len = 3

# 代码

```python
class Solution:
    def longestPalindrome(self, s):
        new_str = '#'
        len_ = [1]
        for c in s:
            new_str += '%c#' % c
            len_.append(0)
            len_.append(0)
        print(len_)
        print(new_str)
        for i in range(1, len(new_str)):
            pre, rear = i-1, i+1
            sub_count = 1
            while pre >=0 and rear < len(new_str):
                if(new_str[pre] == new_str[rear]):
                    sub_count += 1
                    pre -= 1
                    rear += 1
                else:
                    break
            len_[i] = sub_count
        print(len_)
        max_index = len_.index(max(len_))   
        max_len = max(len_)
        res_str = s[int((2 * max_index - (max_index + max_len - 1))/2) : int((max_index + max_len - 1)/2)]
        return res_str

solution = Solution()
print(solution.longestPalindrome('hello'))
```

