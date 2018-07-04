---
layout:     post
title:      "带遮挡的开集人脸识别"
subtitle:   " \"环境搭建\""
date:       2018-06-27 10:00:00
author:     "yang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# 赛题介绍

* 赛事名称： 第五届中国研究生智慧城市技术与创意设计大赛
* 赛题名称： 智能技术挑战赛-智能图片技术-带遮挡的开集人脸识别
* 赛题内容： 在人脸部分被遮挡的情况下，识别出给定的目标人脸图像对应的人物身份。
带遮挡的人脸识别任务可以描述为：给定特定人脸图片集作为probe，并给出指定的gallery图片库，由参评系统自动地在probe中进行人脸识别，并返回该图片识别出为gallery图片库的人脸的ID信息（注意，probe集中的人脸未必包含在gallery集中）。


# 环境搭建

## 安装

anaconda 和 pycharm

## 部署

1. 使用conda新建python环境

```
conda create --name python_3_5 python=3.5 numpy scipy
```

2. 安装boost

[二进制boost](https://sourceforge.net/projects/boost/files/ )下载最新版本，安装到x:\local\xx,路径按照默认

3. 安装dlib

```
conda install -c menpo dlib
```


# 文档

1. [dlib及face_recognition安装指南](https://blog.csdn.net/wyc12306/article/details/79286361)
2. [windows7下安装python库face_recognition](https://blog.csdn.net/Cabchinoe/article/details/78392502)
