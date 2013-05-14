---
layout: post
title: "Mysql的InnoDB存储引擎的分析研究"
description: ""
category: 
tags: [mysql, innodb]
---
{% include JB/setup %}

在使用innodb存储数据的时候发现，数据表中有一行是自增列表（AUTO_INCREMENT），在插入数据成功后，id自动加一，但是当数据插入重复数据，因为我给某个字段设定的是唯一索引，插入失败后再次插入失败id值居然不连续。
比如：
    
    id  username
    1   张三
    2   李四

因为username设定了唯一索引，如果再插入一条重复的记录，是插入不进去的，但是新插入的不重复的记录id就不是从3开始的而是从4开始的。

通过查阅相关资料，得到如下结论：

在这种场景下，InnoDB无法在执行Insert语句之前知道确切的插入记录数，因此会使用表级的AUTO_INC锁（该锁比较特殊，并不像通常的锁那样，在事务结束时释放，而是在该语句执行完毕后释放）。对于AUTO_INCREMENT值，目前InnoDB会采取预分配的策略，即首先分配1，如果用尽则double，如果用尽再double，即1，2，4，8...。需要注意的是，如果innodb_autoinc_lock_mode =2，那么InnoDB不会使用AUTO_INC锁。

在INSERT语句执行之前，InnoDB无法知道数据最终是被插入还是更新，因此可能会导致InnoDB预分配的AUTO_INCREMENT值最终没有被使用。

参考地址

http://www.yiibase.com/mysql/view/460.html

http://dev.mysql.com/doc/refman/5.1/en/innodb-auto-increment-handling.html