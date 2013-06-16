---
layout: post
title: "Centos6.4源码编译安装Nginx1.4.1"
description: "Centos6.4源码编译安装Nginx1.4.1"
category: 
tags: [centos, linux, nginx, 源码编译]
---
{% include JB/setup %}

首先，要解答一个疑问。为什么要在Centos上安装Nginx？答案很简单。非常多的大型网站几乎都在使用Nginx作为WEB服务器或者代理服务器。那么为什么是Centos呢？也很简单，Centos已经成为业内服务器首选安装的操作系统。Nginx只有在Centos或者linux系统下才能发挥它的性能这句话不一定是对的，但是看看大家的选择可以肯定错不了。

####1.源码编译还是yum安装?
Nginx在一些Linux发行版和BSD的各个变种版本的安装包仓库中都会有，通过各个系统自带的软件包管理方法即可安装。需要注意的是，很多预先编译好的安装包都比较陈旧，大多数情况下还是推荐直接从源码编译。**所以，果断源码安装吧**

####2.Nginx的版本选择？
Nginx官网提供了三个类型的版本：Mainline version（Mainline 是 Nginx 目前主力在做的版本，或者可以说是开发版），Stable version（最新稳定版），Legacy versions（遗留的老版本的稳定版）。三个版本到底选择哪一个呢？基于我们使用Nginx大多数情况是在生产环境上使用的，如果要在生产环境上搭建web环境强烈建议使用**Stable version**

####3.安装环境说明
由于进行Centos6.4版本，我选择的是CentOS-6.4-x86_64-minimal.iso。我也强烈建议大家使用这个版本进行安装。理由1.64位操作系统在使用硬盘和内存上没有空间的限制。理由2.最小化安装可以使系统更干净，如果你有强迫症的话，这个肯定是最喜欢的选择。

####综述
其实我的目的只有一个，我们现在做的测试或者试验是作为搭建生产环境的基础。因此，我们安装系统的目的就是为了能够完整的搭建一个生产环境。

*****

####安装步骤

**检查安装Nginx的依赖性，nginx的一些模块需要第三方库的支持，检查是否安装了下列库**

	zlib zlib-level(gzip模块需要 zlib 库)
	openssl openssl-level(ssl 功能需要openssl库)
	prce prce-level(rewrite模块需要 pcre 库)

验证是否安装了以上软件可以使用`rpm -qa | grep xxx`.比如，`rpm -qa | grep zlib`.下面是一个比较通用的yum安装依赖包的命令，你可以直接复制在命令行下执行

    yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel pcre pcre-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers

**下载并安装Nginx**

我们一般会把所有下载的文件放在`/usr/local/src`中。因此，我们先要进入这个目录

    #进入下载目录
    /usr/local/src
    #下载Nginx源码，目前，最稳定的版本是1.4.1
    wget http://nginx.org/download/nginx-1.4.1.tar.gz
    #解压tar包
    tar -zxvf nginx-1.4.1.tar.gz
    #进入nginx源码目录
    cd nginx-1.4.1
    ./configure --prefix=/usr/local/nginx
    make
    make install

这样，我们就可以将nginx安装在了/usr/local/nginx目录中了。安装过后，默认是不启动的，你可以使用以下命令启动Nginx

    /usr/local/nginx/sbin/nginx

启动，可以使用
    
    [root@bogon ~]# ps -ef |grep nginx
    root      9168     1  0 Jun16 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
    nobody    9169  9168  0 Jun16 ?        00:00:00 nginx: worker process      
    root      9267  1374  0 00:55 pts/0    00:00:00 grep nginx

可以看到，nginx已经开始提供服务了。

PS:如果你的网内的其他机器还是没有办法访问你的web服务器，有可能是你的防火墙禁止了80端口，你只需要在`vi /etc/sysconfig/iptables`中增加一行`-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT`,然后重启防火墙`/etc/init.d/iptables restart`

<div align="center"><a href="http://www.flickr.com/photos/94766090@N04/9056951740/" title="Flickr 上 江边望海 的 Nginx1.4.1安装完成" target="_blank"><img src="http://farm4.staticflickr.com/3828/9056951740_23b9d48b79_z.jpg" width="640" height="385" alt="Nginx1.4.1安装完成"></a></div>

如果你能看到这个图，就证明你安装正确了。