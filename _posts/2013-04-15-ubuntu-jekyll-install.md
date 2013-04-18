---
layout: post
title: "在ubuntu搭建jekyll的运行环境"
description: "在ubuntu搭建jekyll的运行环境"
category: 
tags: [ubuntu, jekyll]
---
{% include JB/setup %}

####为什么要在ubuntu上安装jekyll的运行环境？
之前我写过一篇文章[解决联想(Lenovo IdeaPad)-Y450在Ubuntu下安装Nvidia驱动 ](http://jiangbianwanghai.com/2013/04/12/y450-nvidia/)，我的笔记本是使用的ubuntu的系统。而我工作的电脑安装的是windows7。所以，有很多时间是不在公司的，要想写点儿东西还要跑公司那岂不是违背了github移动办公的宗旨，因此就在ubuntu下安装了Jekyll的运行环境。

####安装过程
Jekyll是需要运行在Ruby环境下的，我对Ruby的熟悉程度仅仅是皮毛而已，在网上查了很多资料讲的都非常的复杂什么源码编译，什么套件之类的都非常的麻烦，而且也不知道能不能装成，最后找到一国外网友的博客发表的一篇博文[http://www.garron.me/bits/install-jekyll-on-ubuntu.html](http://www.garron.me/bits/install-jekyll-on-ubuntu.html)帮了我大忙。

**具体总结一下，就是三步：**

1、安装Ruby

    sudo apt-get install ruby1.9.1-dev
    
2、安装Jekyll

    sudo gem install jekyll
    
3、安装rake(用来可以生成markdown文件)

    sudo gem install rake
