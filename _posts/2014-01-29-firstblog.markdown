---
layout:     post
title:      "第一篇博客"
subtitle:   " \"我们的博客开通啦！\""
date:       2016-10-28 12:00:00
author:     "cailang.X"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 生活
---

> “Yeah It's on. ”


# 写在前面

我们的blog就这么开通了，花了大概两天的时间来搭建框架和申请域名,使用的是Hux大神的Theme，这是第一篇blog，以后我们（L.Y）将会一直不定期更新我们的blog，Y主要是写工作上的项目（大部分就是code啦），L主要是写量子信息方面的理解和感悟，希望我们能坚持下去，加油!

---

# GitHub pages + jekyll建立自己的博客

接下来说说搭建这个博客的步骤。  

正好之前就有关注过 [GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/) 快速 Building Blog 的技术方案，非常轻松时尚。

其优点非常明显：

* **Markdown** 带来的优雅写作体验
* 非常熟悉的 Git workflow ，**Git Commit 即 Blog Post**
* 利用 GitHub Pages 的域名和免费无限空间，不用自己折腾主机
	* 如果需要自定义域名，也只需要简单改改 DNS 加个 CNAME 就好了
* Jekyll 的自定制非常容易，基本就是个模版引擎

域名是在阿里云里面申请的，只要6元人民币（第一年），绑定域名的操作可以参考[GitHub Pages 绑定来自阿里云的域名](http://blog.csdn.net/yuan3065/article/details/51594454)



---

配置的过程中也没遇到什么坑，基本就是 Git 的流程，相当顺手

大的 Jekyll 主题上直接 fork 了 Clean Blog（这个主题也相当有名，就不多赘述了。唯一的缺点大概就是没有标签支持，于是我给它补上了。）

本地调试环境需要 `gem install jekyll`，结果 rubygem 的源居然被墙了……后来手动改成了国内的镜像源才成功:

```
gem sources --add http://gems.ruby-china.org/
gem sources --remove https://rubygems.org/
gem install bundler
```

Theme 直接在 [Jekyll Themes](jekyllthemes.org/)里面找就行了，不过之前找的都不太好，后来在知乎里面看到了这个模板，于是乎就用上了，很靠谱。

# 博客编写

博客编写使用过Sublime Text、Markdown Pad和Atom，用过之后果断选择了Atom，狂拽酷炫叼炸天啊，以后发量子相关的博客还要用到mathType这些工具。

——cailang.X 2016-10
