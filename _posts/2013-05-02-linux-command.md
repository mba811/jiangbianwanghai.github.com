---
layout: post
title: "Linux常用命令记录（陆续更新中...）"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}

**df**

df 是来自于coreutils 软件包，系统安装时，就自带的；我们通过这个命令可以查看磁盘的使用情况以及文件系统被挂载的位置；

举例：

    [root@localhost beinan]# df -lh
    Filesystem 容量 已用 可用 已用% 挂载点
    /dev/hda8 11G 6.0G 4.4G 58% /
    /dev/shm 236M 0 236M 0% /dev/shm
    /dev/sda1 56G 22G 35G 39% /mnt/sda1

我们从中可以看到,系统安装在/dev/hda8 ；还有一个56G的磁盘分区/dev/sda1挂载在 /mnt/

*****

**split**

功能说明：切割文件

格式: split \[-n\] file \[name\] 

参数说明： 

-n: 指定截断的每一文件的长度，不指定缺省为1000行 

-b: 按容量划分

file: 要截断的文件 
name： 截断后产生的文件的文件名的开头字母，不指定，缺省为x，即截断后产生的文件的文件名为xaa,xab....直到xzz 
例： 
split -55 myfile ff 
将文件myfile依次截断到名为ffaa,ffab,ffac.....的文件中，每一文件的长度为55行

split -b 300m myfile    将myfile文件按300m进行分割 