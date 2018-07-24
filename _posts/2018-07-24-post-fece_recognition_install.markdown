---
layout:     post
title:      "face_recognition 安装"
subtitle:   " \"机器学习\""
date:       2018-07-24 10:00:00
author:     "lang"
header-img: "http://lyang-blog-pics.oss-cn-shanghai.aliyuncs.com/post-bg-2017/0330/170330.jpg"

catalog: true
tags:
    - Tech
---

# MSVC

下载安装Visual studio 2017 c/c++ 版本，community版本即可
*安装MinGW也可以*

# CMake

下载安装 cmake，注意是64bit

# BOOST

首先，[下载boost](http://www.boost.org/)

一般安装到c:/local目录下

* 打开c:/local，双击 **boostrap.bat**
* 生成了b2.exe
* 打开命令行，定位到c:/local，输入 
    b2 install
* 等待时间有点久
* 编译库文件，命令行输入
    b2 -a --with-python address-model=64 toolset=msvc runtime-link=static
* 设置环境变量
    set BOOST_ROOT=C:\local\boost_X_XX_0
    set BOOST_LIBRARYDIR=C:\local\boost_X_XX_X\stage\lib

# dlib

* 直接在github上同步[dlib源代码](https://github.com/davisking/dlib.git),前提是安装了github客户端
* 定位到dlib根目录，找到setup.py，打开conda prompt （安装了anaconda），输入
    python setup.py install

# face_recognition

dlib安装成功后,在conda prompt直接输入
    pip install face_recognition

----------------------------分割线----------------------------------

以上步骤在有些电脑安装成功过，但是有些电脑会出现 “无法解析的外部符号” 问题，还没找到原因

换成一下命令可以安装

    conda install -c menpo dlib
