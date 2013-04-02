---
layout: post
title: "Linux命令行下操作php的经验总结"
description: "Linux命令行下操作php的经验总结"
category: linux
tagline: "转载本站文章请注明作者和出处"
tags: [linux, php, 导数据]
---
{% include JB/setup %}

随着网站的成长，网站改版也是常事儿。有时候不得不对数据进行迁移，对于从A库转移到B库，从A表转移到B表。我们一般的想法就是写一个PHP脚本然后再浏览器窗口进行循环进行导入。但是浏览器执行php要走dns、http协议这些，无形中处理数据的效率会大打折扣。那何不在linux下面直接执行php脚本呢？

####遇到的问题

**1.如何像在浏览器中那样获得$_GET参数呢？**

需要使用$argv - 传递给脚本的参数数组

    [root@bogon wwwroot]# php test.php page 1 cateid 120
    Array
    (
        [0] => test.php
        [1] => page
        [2] => 1
        [3] => cateid
        [4] => 120
    )

test.php源码

    <?php
    print_r($argv);
    
**2.如何记录php运行日志呢？**

一开始，我是在php中写脚本，将执行完的数据id存储到一个文本中，循环一次就是执行一次文本操作。这种方式虽然可以实现我的要求，但是不够灵活，需要考虑文本文件是否可写。现在的解决办法是让php脚本把记录id echo 出来然后用下面这个命令就可以实现日记记录啦。

    /usr/local/php/bin/php /usr/local/src/work.php > id.log

是不是很方便呢？

**3.如何让php脚本后台执行呢？**

在本机的虚拟机上执行php脚本，下班后电脑一关就没有办法执行了。而导数据是一个慢功夫，可能需要一天一夜，所以一般都是部门内部的服务器上执行php脚本，但是我已关闭终端，脚本就不再执行了。因此，如何关闭终端后脚本还正常执行呢？如上面提到的命令，可以进行一些改造。

    nohup /usr/local/php/bin/php /usr/local/src/work.php > id.log &
    
这样即便是关闭了终端，这个命令也可以作为一个后台进程继续执行下去。知道把数据处理完成为止。

**4.数据转移就是一条条数据的转移肯定是要用到循环的，这循环怎么写呢？这个页码又该如何保持呢？**

先看一段伪代码吧

    while (1)
    {
        if ($page < $count) {
            执行处理的脚本
        } else {
            break;
        }
    }
    
使用while(1)作为死循环，只要不满足跳出的条件就一直执行下去。那个$page每循环一次都要加1的，而这个$page可以存到内存中，一开始我采用的是将$page的值存到memcache里面，虽然可以实现但是，代码的移植性很差，何不把$page的值存到静态变量里面呢，其实静态变量是内存中开了一个空间而已，而且实现起来也比较简单。因为我的到数据脚本是面向过程的，没有专门去研究如何封装，反正也不让别人用自己用着顺手就行了，所以代码质量比较槽，不过能完整需求。我专门写了一个计数器类。代码如下：

    class counter
    {
        public static $counter = 0;
        
        function __construct($counter = 0)
        {
            self::$counter = $counter;
        }
        
        public static function getCounter()
        {
            self::$counter ++;
            return self::$counter;
        }
    }
    
以上内容是我个人的一些总结，可能不够专业，希望给网友指正。