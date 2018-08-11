---
layout:     post
title:      "神奇数"
subtitle:   " \"笔试真题\""
date:       2018-08-11 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

给出一个区间[a, b]，计算区间内“神奇数”的个数。  
神奇数的定义：如果一个数的数字组合可以分成两部分A，B，如果A部分之和等于B部分之和，则这个数是一个神奇数。  
比如：242，存在[2，2]和[4]，2 + 2 = 4，则242是神奇数。  

**输入描述**:  
输入为两个整数a和b，代表[a, b]区间  
**输出描述**:  
输出为一个整数，表示区间内满足条件的整数个数  

    输入例子:
    1 50
    输出例子:
    4

# 思路

先将数字n转成一个数字数组，比如242转换成[2,4,2]，可以分成两组，说明所有数字之和sum_为偶数，只要我们找到了数字之和为sum_/2的组合，则另一个组合也一定是sum_/2。从数组的两头往中间走，每走一步减掉相应的数字，一直到和等于sum_/2。

# 代码

```python
#神奇数
def isMagic(n):
    nums = []
    sum_ = 0
    while n > 0:
        temp = n % 10
        sum_ += temp
        nums.append(temp)
        n -= temp
        n = int(n/10)
    if sum_ % 2 != 0:
        return False
    else:
        #nums.sort()
        low, high = 0, len(nums) - 1
        flag = False
        sum_sub = int(sum_/2)
        cur_sum = sum_
        while low <= high:
            cur_sum -= nums[low]
            low += 1
            if cur_sum == sum_sub:
                flag = True
                break
            cur_sum -= nums[high]
            high -= 1
            if cur_sum == sum_sub:
                flag = True
                break
        return flag

l_, r = [int(i) for i in input().split()]
cnt = 0
if r <= 10:
    print(0)
else:
    for i in range(l_, r+1, 1):
        if isMagic(i):
            cnt += 1
    print(cnt)
```