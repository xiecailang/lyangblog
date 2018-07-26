---
layout:     post
title:      "Z型转换"
subtitle:   " \"coding\""
date:       2018-07-26 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# problem

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
    P   A   H   N
    A P L S I I G
    Y   I   R   
如下形式：

    |  /|
    | / |
    |/  |

# 思路

找出转换后字符位置与其在原字符串中位置的关系

# 代码

```python
class Solution:
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        res_str = ""
        one_len = max(1, numRows + numRows - 2)

        for i in range(0, numRows):
            if i == 0 or i == numRows - 1:
                j = i
                while j < len(s):
                    res_str += s[j]
                    j += one_len
            else:
                j = i
                col = 0
                while j < len(s):
                    res_str += s[j]
                    if(col % 2 == 0):
                        j = j + one_len - i * 2
                    else:
                        j = j + 2 * i
                    col += 1
        return res_str
solution = Solution()
print(solution.convert('A', 1))   
```

