---
layout:     post
title:      "安排机器"
subtitle:   " \"笔试真题\""
date:       2018-08-10 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 问题

题目描述：小Q的公司最近接到m个任务，第i个任务需要Xi的时间去完成，难度等级为yi。  
小Q拥有n台机器，每台机器最长工作时间zi，机器等级wi。  
对于一个任务，它只能交由一台机器来完成，如果安排给它的机器的最长工作时间小于任务需要的时间，则不能完成，  
如果完成这个任务将获得200\*xi + 3\*yi收益。  
对于一台机器，它一天只能完成一个任务，如果它的机器等级小于安排给它的任务难度等级，则不能完成。  
小Q想在今天尽可能的去完成任务，即完成的任务数量最大。如果有多种安排方案，小Q还想找到收益最大的那个方案。小Q需要你来帮助他计算一下。  
输入描述：输入包括 N + M + 1行  
输入的第一行为两个正整数n和m（1 <= n, m <= 100000），表示机器的数量和任务的数量。  
接下来n行，每行两个整数zi和wi（0 < zi < 1000, 0 <= wi <= 100），表示每台机器的最大工作时间和机器等级。  
接下来的m行，每行两个整数xi和yi（0 < xi < 1000, 0 <= yi <= 100），表示每个任务需要的完成时间和任务的难度等级。  
输出描述：输出两个整数，分别表示最大能完成的任务数量和获取的利益。  

    输入： 1 2
    100 3
    100 2
    100 1
    输出：1 20006

# 思路

[引自此处](https://blog.csdn.net/chen134225/article/details/81047868), 从公式 **200\*xi + 3\*yi** 可知时间收益要远大于任务等级收益，因此要获得最大收益，可以将任务和机器先按照时间降序排序，再按照等级降序排序  

1. 遍历任务搜寻可用机器(机器可用时间>=任务时间)，放入到等级列表cnt中
2. 以任务等级为起点，寻找距离任务等级最近的机器进行分配

# 代码

```python
#安排机器
class node:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
            return 'x='+self.x+', y='+self.y
n,m = 3,3
task_ = [node(10, 20), node(90, 56), node(23, 45)]
machine_ = [node(56, 22), node(53, 45), node(98, 12)]
from operator import attrgetter
sorted(task_, key = attrgetter('x','y'), reverse = True)
sorted(machine_, key = attrgetter('x','y'), reverse = True)

cnt = [0]*105
i, j, k, num, ans = 0, 0,0,0,0

for i in range(m):
    while j<n and task_[i].x <= machine_[j].x:
        cnt[machine_[j].y] += 1
        j += 1
    for k in range(task_[i].y, 101,1):
        if cnt[k]:
            num += 1
            cnt[k] -= 1
            ans += (200 * task_[i].x + 3 * task_[i].y)
            break

print(num, ans)
```