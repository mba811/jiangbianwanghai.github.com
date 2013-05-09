---
layout: post
title: "记录tokyo tyrant 安装过程"
description: ""
category: 
tags: [Tokyo Tyrant, Tokyo Cabinet, TTserver, nosql]
---
{% include JB/setup %}

####Tokyo Cabinet & Tokyo Tyrant 介绍
[Tokyo Cabinet](http://fallabs.com/tokyocabinet/)是日本人**平林幹雄**开发的一款 DBM 数据库，该数据库读写非常快，哈希模式写入100万条数据只需0.643秒，读取100万条数据只需0.773秒，是 Berkeley DB 等 DBM 的几倍。

[Tokyo Tyrant](http://fallabs.com/tokyotyrant/)是由同一作者开发的 Tokyo Cabinet 数据库网络接口。它拥有Memcached兼容协议，也可以通过HTTP协议进行数据交换。

Tokyo Tyrant 加上 Tokyo Cabinet，构成了一款支持高并发的分布式持久存储系统，对任何原有Memcached客户端来讲，可以将Tokyo Tyrant看成是一个Memcached，但是，它的数据是可以持久存储的。这一点，跟新浪的Memcachedb性质一样。

**相比Memcachedb而言，Tokyo Tyrant具有以下优势：**

1、故障转移：Tokyo Tyrant支持双机互为主辅模式，主辅库均可读写，而Memcachedb目前支持类似MySQL主辅库同步的方式实现读写分离，支持“主服务器可读写、辅助服务器只读”模式。

2、日志文件体积小：Tokyo Tyrant用于主辅同步的日志文件比较小，大约是数据库文件的1.3倍，而Memcachedb的同步日志文件非常大，如果不定期清理，很容易将磁盘写满。

3、超大数据量下表现出色：

但是，Tokyo Tyrant 也有缺点：在32位操作系统下，作为 Tokyo Tyrant 后端存储的 Tokyo Cabinet 数据库单个文件不能超过2G，而64位操作系统则不受这一限制。所以，如果使用 Tokyo Tyrant，推荐在64位CPU、操作系统上安装运行。

**安装 Tokyo Cabinet**

我习惯将下载的文件放入/usr/local/src中。安装后的程序放在/usr/local/中。我的操作系统是centos6.0 64bits

    cd /usr/local/src/
    wget http://fallabs.com/tokyocabinet/tokyocabinet-1.4.48.tar.gz
    tar zxvf tokyocabinet-1.4.48.tar.gz
    cd tokyocabinet-1.4.48
    ./configure --prefix=/usr/local/tokyocabinet
    make
    makt install
    cd ../

这个过程应该是比较顺利的

**安装 Tokyo Tyrant**

    wget http://fallabs.com/tokyotyrant/tokyotyrant-1.1.41.tar.gz
    tar zxvf tokyotyrant-1.1.41.tar.gz
    cd tokyotyrant-1.1.41
    ./configure --prefix=/usr/local/tokyotyrant --with-tc=/usr/local/tokyocabinet/
    make
    make install
    cd ../

**--with-tc=/usr/local/tokyocabinet/**这个必须带上，这是因为Tokyo Tyrant是Tokyo Cabinet网络接口.如果不添加就会出现“configure: error: tcutil.h is required”