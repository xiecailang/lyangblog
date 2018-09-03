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
<center>$$\mathrm{sign}=\left\{\begin{array}{ll}+1, \& x\leq{}0 \\ -1, \& x<0 \end{array}\right\}$$</center> 
数据要求线性可分，满足对于所有$$y_i = +1$$时，有$$wx_i+b>0$$，对所有$$y_i = -1$$，有$$wx_i+b<0$$

# 学习策略

任意点$$x_0$$到超平面的距离：  
<center>$$\frac{1}{\parallel{}w\parallel}\mid{}wx_0+b\mid$$</center>
其中$$\parallel{}w\parallel$$是向量$$w$$的$$L_2$$范数  
其次，对于 **误分类** 的数据$$(x_i, y_i)$$满足：  
<center>$$-y_i(wx_i+b)>0$$</center>
因此，误分类点到超平面S的距离也可以表示为：  
<center>$$-\frac{1}{\parallel{}w\parallel}y_i(wx_i+b)$$</center>  
那么，假设误分类点集合为M，所有误分类点到超平面S的总距离为：  
<center>$$-\frac{1}{\parallel{}w\parallel}\sum_{x_i\in{}M}^{}y_i(wx_i+b)$$</center>  
如果不考虑范数$$\parallel{}w\parallel$$，可以得到感知机的损失函数为：  
<center>$$L(w, b) = -\sum_{x_i\in{}M}^{}y_i(wx_i+b)$$</center>

# 学习算法

感知机的学习就是基于测试数据，最小化损失函数$$\mathrm{min}L(w, b)$$。选取一个初始超平面$$(w_0, b_0)$$，然后用梯度下降法不断极小化目标函数， **极小化过程中，不是一次使用M中所有的误分类点的梯度下降，而是一次随机选取一个误分类点使其梯度下降**  
如果误分类点集合M是固定的，那么损失函数$$L(w, b)$$的梯度由  
<center>$$\nabla_wL(w, b) = -\sum_{x_i\in{}M}^{}y_ix_i$$</center>  
<center>$$\nabla_bL(w, b) = -\sum_{x_i\in{}M}^{}y_i$$</center>  
给出，随机选取一个误分类点$$(x_i, y_i)$$，对$$w, b$$进行更新：  
<center>$$w \gets w+\eta{}y_ix_i$$</center>  
<center>$$b \gets b+\eta{}y_i$$</center>  
其中，$$eta(0\leq\eta\leq1)$$是步长，也称为学习率。

具体算法步骤：  

1. 选取初始值$$w_0,b_0$$  
2. 在训练集中选取$$(x_i, y_i)$$
3. 如果$$y_i(wx_i+b)\leq 0$$  
<center>$$w \gets w+\eta{}y_ix_i$$</center>  
<center>$$b \gets b+\eta{}y_i$$</center>  

4. 转至步骤2，直到训练集中没有误分类点

# 代码实现

```python
#感知机学习
import random
import numpy as np
import matplotlib.pyplot as plt
class Data:
    def __init__(self, x, y):
        self.x = x
        self.y = y
class Perceptron:
    def __init__(self, w0, b0, data, eta):
        self.w0, self.b0, self.data, self.eta = w0, b0, data, eta
        self.w = w0
        self.b = b0
        self.data_size = len(x)
    def func_filter(self, vals):
        sum_ = np.sum(np.multiply(vals.x, self.w))
        if vals.y * (sum_ + self.b) <= 0:
            return True
        else:
            return False
    def train(self):
        error_data = list(filter(self.func_filter, self.data))
        while len(error_data) > 0:
            index = 0#random.randrange(0, len(error_data))
            sum_ = np.sum(np.multiply(error_data[index].x, self.w))
            if error_data[index].y * (sum_ + self.b) <= 0:
                self.w = self.w + self.eta * error_data[index].y * error_data[index].x
                self.b = self.b + self.eta * error_data[index].y
            error_data = list(filter(self.func_filter, self.data))
        return self.w, self.b
        
        
x = np.array([[3, 3], [4, 3], [1, 1], [2, 2], [2, 3]])
y = np.array([1, 1, -1, -1, -1])
data = []
x_samples = [i[0] for i in x]
y_samples = [i[1] for i in x]
for i in range(len(x)):
    d = Data(x[i], y[i])
    data.append(d)
w0, b0, eta = np.array([0, 0]), 0, 1
perceptron = Perceptron(w0, b0, data, eta)
w, b = perceptron.train()

for i in range(len(x_samples)):
    color_ = 'green' if y[i] > 0 else 'red'
    plt.scatter(x_samples[i], y_samples[i], color=color_)

#w为法向量
k = -w[0]/w[1]
x_line = np.arange(0, 6, 1)
y_line = np.multiply(k, x_line)-b

plt.plot(x_line, y_line)

plt.show()
```