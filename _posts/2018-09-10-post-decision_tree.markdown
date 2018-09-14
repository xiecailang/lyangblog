---
layout:     post
title:      "决策树"
subtitle:   " \"机器学习\""
date:       2018-09-10 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

>决策树是一种基本的分类和回归方法，其优点是具有可读性，分类速度快。决策树学习通常包括3个步骤：特征选择、决策树生成和决策树修剪。相关的算法有ID3, C4.5 以及CART算法

# 特征选择之信息增益

**熵（Entropy）**  
<center>$$H(X) = -\sum_{i = 1}^{n}p_i \log p_i$$</center>  
当$$\log$$取2为底单位为比特(bit)，为自然对数e时单位为纳特(nat)。  

**条件熵（Conditional Entropy）**  
<center>$$H(Y \mid X) = \sum_{i = 1}^{n}p_iH(Y \mid X = x_i)$$</center>  
当出现0概率，令$$0\log 0 = 0$$

**信息增益（information gain）**  
<center>$$g(D, A) = H(D) - H(D \mid A)$$</center>  
D为数据集，A为特征集  
信息增益算法：  
输入：数据集D，特征A  
输出：特征A对数据集的信息增益$$g(D, A)$$  

1. 数据集D经验熵  
<center>$$H(D) = -\sum_{k = 1}^{K}\frac{\mid C_k \mid}{\mid D \mid}\log_2\frac{\mid C_k \mid}{\mid D \mid}$$</center>  
$$C_k$$为数据集的标记，D中包含$$K$$个类，

2. 特征A对D的经验条件熵$$H(D \mid A)  
<center>$$H(D \mid A) = \sum_{i = 1}^{n}\frac{\mid D_i \mid}{\mid D \mid}H(D_i) = -\sum_{i = 1}^{n}\frac{\mid D_i \mid}{\mid D \mid}\sum_{k = 1}^{K}\frac{\mid D_{ik} \mid}{\mid D_i \mid}\log_2\frac{\mid D_{ik} \mid}{\mid D_i \mid}$$</center>  
特征A将数据集分成了n个子集$$D_i$$，在$$D_i$$中属于类$$C_k$$的个数为$$\mid D_{ik} \mid$$  

**信息增益比（information gain ratio）**  
<center>$$g_R(D, A) = \frac{g(D, A)}{H_A(D)}$$</center>  
其中$$H_A(D) = -\sum_{i = 1}^{n} \frac{\mid D_i \mid}{\mid D \mid}\log_2\frac{\mid D_i \mid}{\mid D_i \mid}$$

# 决策树构建

**ID3算法** 递归创建决策树步骤  
输入：训练集D，特征集A，阈值e  
输出：决策树T  

1. 若 D 中所有实例属于同一类$$C_k$$，则 T 为单节点树，将$$C_k$$作为该节点的类标记，返回T
2. 若A为空集，则T为单节点树，并将D中实例数最大的类$$C_k$$作为该节点的类标记，返回T
3. 否则，**选择信息增益最大的特征**$$A_g$$，如果$$A_g$$的信息增益小于阈值e，则T为单节点树，并将D中实例数最大的类$$C_k$$作为该节点的类标记，返回T
4. 否则，对$$A_g$$的每一个可能的值$$a_i$$将测试集D分为若干个非空子集$$D_i$$,将$$D_i$$中实例数最大的类作为标记，构建子节点，由节点和子节点构成树T，返回T
5. 对于第$$i$$个子节点，以$$D_i$$为训练集，以$$A - {A_g}$$为特征集，递归调用步骤1-4，得到子树$$T_i$$，返回$$T_i$$
   
**C4.5算法** 将ID3算法中的特征选择改成了 **信息增益比**

# 构造决策树代码

```python
#决策树
#特征空间
A = ['age', 'work', 'house', 'credit']
A_detail = {'age': 3, 'work': 2, 'house':2, 'credit':3}
C = [0, 1]
src_data = [{'age': 0, 'work': 0, 'house':0, 'credit':0}, {'age': 0, 'work': 0, 'house':0, 'credit':1},{'age': 0, 'work': 1, 'house':0, 'credit':1},{'age': 0, 'work': 1, 'house':1, 'credit':0},{'age': 0, 'work': 0, 'house':0, 'credit':0},
{'age': 1, 'work': 0, 'house':0, 'credit':0},{'age': 1, 'work': 0, 'house':0, 'credit':1},{'age': 1, 'work': 1, 'house':1, 'credit':1},{'age': 1, 'work': 0, 'house':1, 'credit':2},{'age': 1, 'work': 0, 'house':1, 'credit':2},
{'age': 2, 'work': 0, 'house':1, 'credit':2},{'age': 2, 'work': 0, 'house':1, 'credit':1},{'age': 2, 'work': 1, 'house':0, 'credit':1},{'age': 2, 'work': 1, 'house':0, 'credit':2},{'age': 2, 'work': 0, 'house':0, 'credit':0}]
src_label = [0,0,1,1,0,0,0,1,1,1,1,1,1,1,0]
uni_label = list(set(src_label))
#数据集经验熵H(D)
h = 0
import math
for i in set(src_label):
    h += src_label.count(i)/len(src_data)*math.log2(src_label.count(i)/len(src_data))
h = -h
import re
#特征A对数据集经验条件熵H(D|A)
def get_conditional_entropy(data, labels, feature):
    #使用特征将子集划分
    t_sum = 0
    h_a = 0
    for i in range(A_detail[feature]):
        sub_sum = 0
        d_i = [index for index,d in enumerate(data) if d[feature] == i]
        labels_i = [src_label[index] for index in d_i]
        d_i_len = len(d_i)
        h_a += d_i_len/len(src_data)*math.log2(d_i_len/len(src_data))
        for j in range(len(uni_label)):
            d_jk = labels_i.count(j)
            if d_jk != 0:
                sub_sum += d_jk/d_i_len*math.log2(d_jk/d_i_len)
        t_sum += d_i_len/len(src_data)*sub_sum
    return -t_sum, -h_a
gain_set = {}
gain_ratio = {}
for s in A:
    gain_set[s], h_a = get_conditional_entropy(src_data, src_label, s)
    gain_set[s] = h - gain_set[s]
    gain_ratio[s] = gain_set[s]/h_a

#ID3构建决策树
import numpy as np
from collections import Counter
class Node:
    def __init__(self):
        self.data = None
        self.feature = None
        self.mark = None
        self.son = []
def print_tree(head):
    p = head
    print('[%s,%d]' % (head.feature if head.feature != None else 'None', head.mark if head.mark != None else -1 ), end = ';')
    if len(head.son) > 0:
        for i in range(len(head.son)):
            print_tree(head.son[i])
#广度优先
from queue import Queue
def print_tree_hor(head):
    q = Queue()
    q.put(head)
    cur = head
    last = cur
    nlast = None
    while not q.empty():
        cur = q.get()
        print(len(cur.data), '[%s,%d]' % (cur.feature if cur.feature != None else 'None', cur.mark if cur.mark != None else -1 ), end = '    ')
        if len(cur.son) > 0:
            for h in cur.son:
                q.put(h)
                nlast = h
        if cur == last:
            print()
            last = nlast
def create_tree(data, labels, feature, gain, ep):
    #数据集属于同一类，直接返回标记
    head = Node()
    head.data = data
    if len(set(labels)) == 1:
        head.mark = labels[0]
        return head
    #特征集为空集，找data中实例数最大的一类作为标记
    if len(feature) == 0:
        if len(labels) > 0:
            head.mark = dict(zip(*np.unique(labels, return_counts = True)))
        return head
        #head.mark = max(labels.count(i) for i in list(set(labels)))
    #选择信息增益最大的进行分割
    max_gain = gain[0]
    max_feature = feature[0]
    if max_gain < ep:
        head.mark = dict(zip(*np.unique(labels, return_counts = True)))
        return head
    else:
        head.feature = max_feature
        for i in range(A_detail[max_feature]):
            d_i = [index for index,d in enumerate(data) if d[max_feature] == i]
            labels_i = [labels[index] for index in d_i]
            son_data = [data[index] for index in d_i]
            head.son.append(create_tree(son_data, labels_i, feature[1:], gain[1:], ep))
        return head

#将gain排序，生成数组
import operator
sort_gain = sorted(gain_set.items(), key = operator.itemgetter(1), reverse = True)
sort_gain_ratio = sorted(gain_ratio.items(), key = operator.itemgetter(1), reverse = True)

#ID3算法，gain作为特征选择依据
gain_lst = [i[1] for i in sort_gain]
feature_lst = [i[0] for i in sort_gain]
head = create_tree(src_data, src_label, feature_lst, gain_lst, ep=0)
print_tree_hor(head)
#C4.5算法，gain_ratio作为特征选择依据
gain_ratio_lst = [i[1] for i in sort_gain_ratio]
ratio_feature_lst = [i[0] for i in sort_gain_ratio]
head_ratio = create_tree(src_data, src_label, ratio_feature_lst, gain_ratio_lst, ep=0)
print_tree_hor(head_ratio)

'''output
15 [house,-1]
9 [credit,-1]    6 [None,1]
4 [None,0]    4 [work,-1]    1 [None,1]
2 [None,0]    2 [None,1]

15 [house,-1]
9 [work,-1]    6 [None,1]
6 [None,0]    3 [None,1]
'''
```

# CART（classification and regression tree）算法

**分类树** 和 **回归树**  

1. 分类树，使用最大熵的一半，或者基尼指数最小作为特征选择的分割点
2. 回归树，使用最小化均方差作为衡量标准

基尼指数：$$\mathrm{Gini}(p) = \sum_{k = 1}^{K}p_k(1-p_k) = 1 - \sum_{k = 1}^{K}p_k^2$$  
均方差：$$\sum_{x_i \in R_m}(y_i - f(x_i))^2$$  

详细参考[决策树(分类树、回归树)](https://blog.csdn.net/weixin_36586536/article/details/80468426)

# 剪枝

过程：首先从生成算法产生的决策树$$T_0$$底端开始不断剪枝，直到根节点，形成一个子树序列$${T_0, T_1, ..., T_n}$$，然后通过交叉验证法在独立的验证数据集上对子树序列进行测试，从中选择最优子树  
计算子树的损失函数：$$C_\alpha = C(T) + \alpha \mid T \mid$$  
$$T$$为任意子树，$$C(T)$$为对训练参数的预测误差（比如基尼指数）,$$\mid T \mid$$为子树的叶节点个数，$$\alpha \geq 0$$为参数  
算法过程：  
输入：CART算法生成的决策树$$T_0$$  
输出：最优决策树$$T_\alpha$$  

1. $$k = 0, T = T_0$$
2. $$\alpha = +\infty$$
3. 自下而上对内部节点$$t$$计算$$C(T_t), \mid T_t \mid$$以及

<center>$$g(t) = \frac{C(t) - C(T_t)}{\mid T_t \mid - 1}$$</center>  
<center>$$\alpha = min(\alpha, g(t))$$</center>  

4. 对$$g(t) = \alpha$$的内部节点$$t$$进行剪枝，并对叶节点$$t$$用多数表决法决定其分类，得到树$$T$$
5. $$k = k + 1, \alpha_k = \alpha, T_k = T$$
6. 如果$$T_k$$不是由根节点和两个叶节点构成的树，则回到步骤3；否则令$$T_k = T_n$$
7. 采用交叉验证法在子树序列$$T_0, T_1, ..., T_n$$中选择最优子树$$T_\alpha$$