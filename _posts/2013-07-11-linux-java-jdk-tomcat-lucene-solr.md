---
layout: post
title: "linux系统环境下搭建solr(lucene)开发环境"
description: "linux系统环境下搭建solr(lucene)开发环境"
category: 
tags: [linux, java, jdk, tomcat, lucene, solr]
---
{% include JB/setup %}

####前言
目前，WEB网站提供搜索引擎服务的解决方案一般常用的选择有两种。基于php开发的网站一般采用sphinx的居多。基于java开发的网站一般采用solr(lucene)的居多。作为数据量和访问量不是很大的网站使用php+sphinx组合提供垂直搜索已经能很好的满足日常需要了，但是数据量、访问量、业务需求（比如，站内推荐）的网站来说，使用solr(lucene)是必然的。

####初学者的疑问
作为一个以前没有一点儿java基础的初学者来说，会有很多疑问。这些是我的疑问，可能也是大家的疑问。

**solr和lucene之间的关系**

lucene其实和其他包一样就是java的一个开发。只有下载了开发包进入到编写的程序中才能使用它的功能。solr是基于lucene开发的封装了很多功能的应用，可以提供管理lucene和提供api接口。

**Windows or Linux**

是啊，在网上找了快一天全都是在windows环境下测试的教程和视频。你妈，入门也真不是这么入门的。你会将lucene安装在windows上作为生产环境使用吗？既然要讲究应该更接近于实战太对。我的目标很强烈，学习它就是想在生产环境用它，所以，强烈推荐centos作为安装配置的环境。

**lucene和solr到底该怎么入手学习呢**

直接配置环境，直接调试程序，调试出来再进行相应的知识学习比一开始上来将神马高端的知识要管用的多。总的，说来安装步骤是这样的：第一步，要有centos操作系统环境；第二步，要安装jdk，因为有了jdk在可以执行java程序；第三步，下载lucene，直接使用提供的测试功能在命令行调试成功通过再考虑在程序中引用它；第四部，安装tomcat，因为咱们使用它是需要提供服务的，而且solr有一个WEB管理界面也需要用到它；第五部，下载solr并调试通过。

--未完待续