---
layout: post
title: "httpcws分词服务的研究"
description: "httpcws分词服务的研究"
category: 
tags: [httpcws, ICTCLAS, 分词]
---
{% include JB/setup %}

提到分词服务，张宴的httpcws和中科院的ictclas是国内比较有名的两个软件。我对httpcws做了一些比较感兴趣的研究，分享一下我的研究成果：

**httpcws是基于ictclas的二次开发**

httpcws在dict/Data/包含有中科院的分词数据模块。httpcws只是通过他们提供的api来处理分词。

**httpcws_dict.txt词库文件其实是一个用户自定义词库**

在不载入httpcws_dict.txt词库文件的情况下，常用的语句也能非常好的进行拆分。比如，“今天早上一大早就去4s店提了一辆宝马5系汽车”，拆分的结果是“今天 早上 一大早 就 去 4s 店 提 了 一 辆 宝马 5 系 汽车”。可以看到“今天”、“早上”、“一大早”、“宝马”、“汽车”都可以划分出来。而“4s店”、“5系”我们可以将这两个词加入到httpcws_dict.txt来优化拆分结果。