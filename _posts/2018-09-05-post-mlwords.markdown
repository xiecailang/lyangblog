---
layout:     post
title:      "携程笔试机器学习概念解析"
subtitle:   " \"机器学习-自然语言处理\""
date:       2018-09-05 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# TF-IDF

参考[TF-IDF及其算法](https://blog.csdn.net/sangyongjia/article/details/52440063)  
TF-IDF (term frequency-inverse document frequency)一般用于资讯检索与资讯勘探的加权技术，**字词的重要性随它在文件出现次数正比增加，但同时也随着它在语料库中出现的频率反比下降**，TF-IDF加权的各种形式经常被搜索引擎应用，作为文件与用户查询之间相关程度的度量或评级。  
**TF** 词频，某一个给定词在该文件中出现的次数，一般会被归一化防止它偏向长文件  
<center>$$tf_{i,j} = \frac{n_{i, j}}{\sum_{k}^{}n_{k, j}}$$</center>  
其中$$n_{i, j}$$表示该词$$t_i$$在文件$$d_j$$中出现的次数，而分母表示文件$$d_j$$所有字词出现次数之和  
**IDF** 逆向文件频率，一个词语普遍重要性的度量，它可以由总文件数目除以包含该词的文件数目，再将该结果取对数得到  
<center>idf_i = \log\frac{\mid D \mid}{\mid \{ j:t_i \in d_j\} \mid}</center>  
其中$$\mid D \mid$$ 表示语料库中文件的总数，分母表示包含词语$$t_j$$的文件数目，为了防止分母出现0，一般使用$$1 + \mid \{ j:t_i \in d_j\} \mid$$做分母  
然后可得  
<center>tfidf_{i, j} = tf_{i,j} \times idf_i</center>

# HMM模型和Viterbi算法

HMM (Hidden Markov Model)隐马尔可夫模型，一般用来解决大多数 **自然语言处理问题**  
Viterbi算法用于解决HMM的搜索最短路径问题

# 众数

一组数据中出现次数最多的数

# Xavier初始化方法

参考[深度学习——Xavier初始化方法](https://blog.csdn.net/shuzfan/article/details/51338178)
Xavier初始化方法是一种很有效的神经网络初始化方法，为了使得网络中信息更好的流动，每一层输出的方差应该尽量相等。基于这个目标，现在我们就去推导一下：每一层的权重应该满足哪种条件。

# Encoder-Decoder

参考[深度学习方法（八）：自然语言处理中的Encoder-Decoder模型，基本Sequence to Sequence模型](https://blog.csdn.net/xbinworld/article/details/54605408)
Encoder-Decoder并不是一个具体的模型，而是一个框架。比如无监督算法的auto-encoding就是用编码-解码的结构设计并训练的；比如这两年比较热的image caption的应用，就是CNN-RNN的编码-解码框架；再比如神经网络机器翻译NMT模型，往往就是LSTM-LSTM的编码-解码框架  
所谓的编码，就是将输入序列转化成一个固定长度的向量；解码，就是将之前生成的固定向量再转化成输出序列

# Attention Model

参考[深度学习笔记——Attention Model（注意力模型）学习总结](https://blog.csdn.net/mpk_no1/article/details/72862348)  
以及[深度学习中的注意力机制](https://blog.csdn.net/tg229dvt5i93mxaq5a6u/article/details/78422216)  
深度学习里的AM (Attention model) 其实模拟的是人脑的注意力模型，举个例子来说，当我们观赏一幅画时，虽然我们可以看到整幅画的全貌，但是在我们深入仔细地观察时，其实眼睛聚焦的就只有很小的一块，这个时候人的大脑主要关注在这一小块图案上，也就是说这个时候人脑对整幅图的关注并不是均衡的，是有一定的 **权重区分**的。这就是深度学习里的Attention Model的核心思想。  
没有引入注意力的模型在输入句子比较短的时候问题不大，但是如果输入句子比较长，此时所有语义完全通过一个中间语义向量来表示，单词自身的信息已经消失，可想而知会丢失很多细节信息，这也是为何要引入注意力模型的重要原因

# word2vec

参考[https://blog.csdn.net/mylove0414/article/details/61616617](https://blog.csdn.net/mylove0414/article/details/61616617)  
word2vec也叫word embeddings,意为 **词向量**，作用就是将自然语言中的字词转为计算机可以理解的稠密向量（Dense Vector）。在word2vec出现之前，NPL通常把字词转为离散的向量，也就是One-Hot Encoder,比如，"我"=[0, 0, ..., 1], "你" = [0, 1, ..., 0]，这个向量就是字词在语料库中的位置。如果语料库太大的话，矩阵过于稀疏，并且会造成 **维度灾难** （随着学习器的维度增加，分类器性能逐步上升，到达某个点后，其性能开始下降，参考[https://blog.csdn.net/zbc1090549839/article/details/38929215](https://blog.csdn.net/zbc1090549839/article/details/38929215)）  
word2vec主要分为 **CBOW（Continuous Bag of Words）** 和 **Skip-Gram** 两种模式。CBOW是从原始语句推测目标字词；而Skip-Gram正好相反，是从目标字词推测出原始语句。CBOW对小型数据库比较合适，而Skip-Gram在大型语料中表现更好。 

# MEMM模型

参考[HMM,MEMM,CRF模型的比较](https://blog.csdn.net/happyzhouxiaopei/article/details/7960876)  
HMM, MEMM (最大熵马尔可夫模型), CRF（条件随机场）这三个模型都可以用来做序列标注模型。但是其各自有自身的特点，HMM模型是对转移概率和表现概率直接建模，统计共现概率。而MEMM模型是对转移概率和表现概率建立联合概率，统计时统计的是条件概率。MEMM容易陷入局部最优，是因为MEMM只在局部做归一化，而CRF模型中，统计了全局概率，在做归一化时，考虑了数据在全局的分布，而不是仅仅在局部归一化，这样就解决了MEMM中的标记偏置的问题。

# bagging, boosting

参考[Bagging和Boosting 概念及区别](https://www.cnblogs.com/liuwu265/p/4690486.html)  
Bagging和Boosting都是将已有的分类或回归算法通过一定方式组合起来，形成一个性能更加强大的分类器，更准确的说这是一种分类算法的组装方法。即将弱分类器组装成强分类器的方法  
他们之间的区别  

方法 | 样本选择 | 权重 | 预测函数 | 并行计算
Bagging | 训练集是在原始集中有放回选取的，从原始集中选出的各轮训练集之间是独立的 | 使用均匀取样，每个样例的权重相等 | 所有预测函数的权重相等 | 各个预测函数可以并行生成
Boosting | 每一轮的训练集不变，只是训练集中每个样例在分类器中的权重发生变化。而权值是根据上一轮的分类结果进行调整 | 根据错误率不断调整样例的权值，错误率越大则权重越大 | 每个弱分类器都有相应的权重，对于分类误差小的分类器会有更大的权重 | 各个预测函数只能顺序生成，因为后一个模型参数需要前一轮模型的结果

# SVD

参考[SVD分解技术详解](http://www.cnblogs.com/peizhe123/p/5113357.html)  
**奇异值分解**，将一个比较复杂的矩阵用更小更简单的几个子矩阵的相乘来表示，这些小矩阵描述的是矩阵的重要的特性。就像是描述一个人一样，给别人描述说这个人长得浓眉大眼，方脸，络腮胡，而且带个黑框的眼镜，这样寥寥的几个特征，就让别人脑海里面就有一个较为清楚的认识，实际上，人脸上的特征是有着无数种的，之所以能这么描述，是因为人天生就有着非常好的抽取重要特征的能力，让机器学会抽取重要的特征  
在机器学习领域，有相当多的应用与奇异值都可以扯上关系，比如做feature reduction的PCA，做数据压缩（以图像压缩为代表）的算法，还有做搜索引擎语义层次检索的LSI（Latent Semantic Indexing）

# 生成模型

参考[生成模型（Generative）和判别模型（Discriminative）](https://www.cnblogs.com/realkate1/p/5683939.html)
Generative Modeling是由训练数据学习联合概率分布P(X,Y)，然后求出条件概率分布P(Y|X)作为预测的模型。之所以叫生成模型，是因为模型表示了给定输入X产生输出Y的生成关系  
Discriminative Modeling（判别模型）是由训练数据直接学习决策函数f(X)或者条件概率分布P(X,Y)作为预测的模型，模型关心的是对给定的输入X，应该预测什么样的输出Y，与GM的不同在于不需要先学习出联合分布P(X,Y)

* 典型的生成模型有：朴素贝叶斯和隐马尔科夫模型
* 典型的判别模型有：k近邻法、感知机、决策树、逻辑回归、最大熵、SVM、AdaBoost和条件随机场等



