---
layout:     post
title:      "感知机学习"
subtitle:   " \"机器学习\""
date:       2018-09-03 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

>感知机是二类分类模型，对输入的samples输出+1或者-1

# 模型

感知机模型表示为：
<center>$$f(x) = \mathrm{sign}(wx+b)$$</center>
其中$$w,b$$是感知机模型参数，分别表示分割数据的超平面的法向量和截距，学习的过程就是通过训练测试数据得到最优的$$w,b$$。  
<center>$$\mathrm{sign}=\left\{\begin{array}{ll}+1, \& x\geqslant{}0 \\ -1, \& x<0 \end{array}\right\}</center> 
数据要求线性可分，满足对于所有$$y_i = +1$$时，有$$wx_i+b>0$$，对所有$$y_i = -1$$，有$$wx_i+b<0$$

# 学习策略

任意点$$x_0$$到超平面的距离：  
<center>$$\frac{1}{\parallel{}w\parallel}\mid{}wx_0+b\mid$$</center>
其中$$\parallel{}w\parallel$$是向量$$w$$的$$L_2$$范数  
其次，对于 **误分类** 的数据$$(x_i, y_i)$$满足：  
<center>$$-y_i(wx_i+b)>0$$</center>
