---
layout: post
title: "时区解析及如何调整Linux下的时区"
description: "科普一下关于时区的知识及如何调整LinuxI下的时区"
category: linux
tags: [linux, 时区]
---
{% include JB/setup %}

####前言
今天发现Vmware中的Centos 6.0-64bit的系统时间和宿主机的时间相差好几个小时，调整系统时间后重新启动又给恢复过去了。于是就想弄明白到底是什么原因。时区又是什么，虽然这个基本常识在初中的地理课上有讲到，出国旅游都要倒时差，中国我都没有跑遍更别提出国了，当时真的没有太注意它的作用。现在既然工作中用到了就恶补一下吧。

####几个关键词

**世界时**

世界时是最早的时间标准。在1884年，国际上将1s确定为全年内每日平均长度的1/8.64×104。以此标准形成的时间系统，称为世界是，即 UT1。1972年国际上开始使用国际原子时标，从那以后，经过格林威治老天文台本初子午线的时间便被称为世界时，即UT2，或称格林威治时间 （GMT），是对地球转速周期性差异进行校正后的世界时。 

**原子时**
 
1967年，人们利用铯原子振荡周期极为规律的特性，研制出了高精度的原子时钟，将铯原子能级跃迁辐射9192631770周所经历的时间定为 1s。现在用的时间就是1971年10月定义的国际原子时，是通过世界上大约200多台原子钟进行对比后，再由国际度量衡局时间所进行数据处理，得出的统 一的原子时，简称TAI。 

**世界协调时**

世界协调时是以地球自转为基础的时间标准。由于地球自转速度并不均匀，并非每天都是精确的86400原子s，因而导致了自转时间与世界时之间存在 18个月有1s的误差。为纠正这种误差，国际地球自转研究所根据地球自转的实际情况对格林威治时间进行增减闰s的调整，与国际度量衡局时间所联合向全世界 发布标准时间，这就是所谓的世界协调时（UTC:Coordinatde Universal Time）。UTC的表示方式为：年（y）、月（m）、日（d）、时（h）、分（min）、秒（s），均用数字表示。

**GMT**

GMT 是 Greenwich Mean Time 的缩写，译为中文为“格林威治标准时间”或“格林尼治标准时间”，直译的话，可译为“格林威治平时”或“格林尼治平时”。这里的格林威治位于英国伦敦东南方向的泰晤士河畔。总的来讲，它的出现就是为规范全球各地区的时间，各国都以一个固定时间为参照点来计算当地时间。为此1884年美国华盛顿特区召开国际经线大会，确定格林威治就是东西经零度的地方。人们将地球人为的分为24等份，每一等份为一个时区，每时区横跨经度15度，即时间上正好是1小时，以格林威治所在地的时区为中时区，我们现在假设GMT时间为1月10日凌晨（即0点整），往西一个时区，则减去一小时，则当地时间是1月9日夜晚23点整，往东一个时区，则加上一小时，则当地时间是1月10日凌晨1点，依次类推，中国在东经120度上，可以这样计算，（东经120°-东经0°）所得度数再除以15，即得8，所以中国在东八区上，那么，此时中国就是1月10日早上8点整。

**UTC**

UTC 是 Coordinated Universal Time 的缩写，译为中文为“世界标准时间”，直译的话，可译为“协调通用时间”或“协调世界时间”。目前来说也就是指 GMT 时间。为什么说目前就是指 GMT 时间呢？因为本初子午线（子午线即经线，本初子午线即 0 度经线）其实穿过的是沙特阿拉伯西边的麦加，而不是英国的格林威治。当时英国皇家学会暂时确定格林威治为本初子午线的穿过点﹐加之英国正是兴旺发达时期，全世界就将错就错，用到现在。说不定哪天改为麦加时间为标准时间也不是没有可能。所以我们一般使用 UTC，而不是 GMT。

**PDT**

PDT 是 Pacific Daylight Time 的缩写，译为中文为“太平洋夏令时间”，比 UTC 时间晚 7 小时。Google 曾在其 AdSense 中提到“在 09-6-13，我们的工程师会在 PDT 大约 上午10:00 到 下午2:00 对网站进行维护。”

**PST**

美国夏季始于每年4月的第1个周日，止于每年10月的最后一个周日。夏令时比正常时间早一小时，与PDT时间相对应的是PST,Pacific Standard Time。 夏时制结束后就是PST。PST是太平洋标准时间（西八区），与北京时间（东八区）时差-16个小时，也就是北京时间减去16就是PST时间。而PDT比PST早1个小时，就是说PDT与北京时间时差为-15小时

其实除了 PDT，PST 时间，北美地区还有很多时间：
<table class="table table-bordered table-striped table-hover">
   <tr>
      <th>缩写</th>
      <th>全称</th>
      <th>中文说明</th>
   </tr>
   <tr>
      <td>NST</td>
      <td>Newfoundland Standard Time</td>
      <td>纽芬兰标准时间</td>
   </tr>
   <tr>
      <td>NDT</td>
      <td>Newfoundland Daylight Time</td>
      <td>纽芬兰夏令时间</td>
   </tr>
   <tr>
      <td>AST</td>
      <td>Atlantic Standard Time</td>
      <td>大西洋标准时间</td>
   </tr>
   <tr>
      <td>ADT</td>
      <td>Atlantic Daylight Time</td>
      <td>大西洋夏令时间</td>
   </tr>
   <tr>
      <td>EST</td>
      <td>Eastern Standard Time</td>
      <td>东部标准时间</td>
   </tr>
   <tr>
      <td>EDT</td>
      <td>Eastern Daylight Time</td>
      <td>东部夏令时间</td>
   </tr>
   <tr>
      <td>CST</td>
      <td>Central Standard Time</td>
      <td>中部标准时间</td>
   </tr>
   <tr>
      <td>CDT</td>
      <td>Central Daylight Time</td>
      <td>中部夏令时间</td>
   </tr>
   <tr>
      <td>MST</td>
      <td>Mountain Standard Time</td>
      <td>山地标准时间</td>
   </tr>
   <tr>
      <td>MDT</td>
      <td>Mountain Daylight Time</td>
      <td>山地夏令时间</td>
   </tr>
   <tr>
      <td>PST</td>
      <td>Pacific Standard Time</td>
      <td>太平洋标准时间</td>
   </tr>
   <tr>
      <td>PDT</td>
      <td>Pacific Daylight Time</td>
      <td>太平洋夏令时间</td>
   </tr>
   <tr>
      <td>AKST</td>
      <td>Alaska Standard Time</td>
      <td>阿拉斯加标准时间</td>
   </tr>
   <tr>
      <td>AKDT</td>
      <td>Alaska Daylight Time</td>
      <td>阿拉斯加夏令时间</td>
   </tr>
   <tr>
      <td>HAST</td>
      <td>Hawaii-Aleutian Standard Time</td>
      <td>夏威夷-阿留申标准时间</td>
   </tr>
   <tr>
      <td>HADT</td>
      <td>Hawaii-Aleutian Daylight Time</td>
      <td>夏威夷-阿留申夏令时间</td>
   </tr>
</table>

*****

####Linux下时区调整

查看当前时区

    [root@bogon ~]# date -R
    Tue, 09 Apr 2013 20:04:40 +0800
    
复制相应的时区文件，替换系统时区文件；或者创建链接文件

    格式：cp /usr/share/zoneinfo/$主时区/$次时区 /etc/localtime
    方法一：cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    方法二：ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

Ps:我就是采取第二种方式调整好centos中的时间的。

*****

**参考链接**

[http://jeffyyko.blog.51cto.com/28563/140004](http://jeffyyko.blog.51cto.com/28563/140004)

[http://lanlfeng.blog.51cto.com/337014/124738](http://lanlfeng.blog.51cto.com/337014/124738)

**本文只是对我遇到的问题进行了简单研究，如有不对或者涉及不到的地方还请在下方留言，我保证有留言必回复**