---
layout: post
title: "centos下让nginx支持ngx_lua_module模块"
description: "ngx_lua_module 是一个nginx的http模块，它把 lua 解析器内嵌到 nginx中，用来解析并执行lua 语言编写的网页后台脚本"
category: nginx
tags: [centos, nginx, ngx_lua_module, lua]
---
{% include JB/setup %}

####前言

ngx_lua_module是一个Nginx的http模块它把Lua解析器内嵌到Nginx中，从而可以让用户在Nginx 核心中直接运行Lua语言编写的程序。我们可以选择在 Nginx不同的请求处理阶段插入我们的Lua代码。这些Lua代码既可以直接内联在Nginx配置文件中，也可以单独放置在外部.lua文件里，然后在 Nginx 配置文件中引用.lua文件的路径。

最先将Nginx，Lua组合到一起的是[OpenResty](http://openresty.org/)，它有一个ngx_lua模块，将Lua嵌入到了Nginx里面。那么为什么要这么做呢？因为：

 - 支持Windows和Linux平台。
 - 支持高并发高性能。
 - HTML网页中内嵌LUA脚本代码，类似于PHP。
 - 支持非阻塞的数据库操作，目前只支持MYSQL。
 - 支持异步的文件IO操作。
 - 支持非阻塞的SOCKET IO操作。
 
####安装

这个地方只介绍在linux下的安装方法。前提是我们已经源码编译过了Nginx并且Nginx的安装路径是`/usr/local/nginx`，我们要做的就是将lua模块编译到Nginx中。所需要的模块如下：

 - [luajit](http://luajit.org)
 - [ngx_devel_kit](https://github.com/simpl/ngx_devel_kit)
 - [echo-nginx-module](https://github.com/agentzh/echo-nginx-module)
 - [lua-nginx-module](https://github.com/chaoslawful/lua-nginx-module)

**1.安装luajit**

    wget http://luajit.org/download/LuaJIT-<version>.tar.gz
    tar zxvf LuaJIT-<version>.tar.gz
    cd LuaJIT-<version>
    make 
    sudo make install PREFIX=/usr/local/luajit

下面需要配置一下 luajit 或 lua 的环境变量(Nginx编译时需要)：

    export LUAJIT_LIB=/path/to/luajit/lib
    export LUAJIT_INC=/path/to/luajit/include/luajit-2.0

**2.安装 ngx_devel_kit (NDK) 模块**

    cd /usr/local
    git clone https://github.com/simpl/ngx_devel_kit.git
下载完成后，将在 /usr/local/ 目录下生成子目录 ngx_devel_kit。

**3.安装 lua-nginx-module 模块**

    cd /usr/local
    git clone https://github.com/chaoslawful/lua-nginx-module.git
    
下载完成后，将在 /usr/local/ 目录下生成子目录 lua-nginx-module。

**4.重新编译Nginx，需要注意编译顺序！**

    ./configure --prefix=/usr/local/nginx \
                --with-ld-opt="-Wl,-rpath,$LUAJIT_LIB" \
                --add-module=/usr/local/ngx_devel_kit \
                --add-module=/usr/local/echo-nginx-module \
                --add-module=/usr/local/lua-nginx-module
    make -j2
    make install

重新编译 Nginx 二进制，Nginx 需要 quit 再启动。而普通配置更新则 reload 即可：

    kill -HUP `cat /path/nginx/logs/nginx.pid`
    /usr/local/nginx/sbin/nginx -s reload

模块编译成功！
重启Nginx服务器！

**5.测试代码**

在Nginx.conf 配置文件中，加入以下代码：

    location /echo {
        default_type 'text/plain';
        echo 'hello echo';
    }

    location /lua {
        default_type 'text/plain';
        content_by_lua 'ngx.say("hello, lua")';
    }

重启Nginx服务器！

    /usr/local/nginx/sbin/nginx -s reload
    
使用curl测试：

    [root@localhost] curl http://localhost/echo
    hello echo
    [root@localhost] curl http://localhost/lua
    hello lua
    
测试结果表明，两个模块都安装成功！
    
**参考地址：**

[http://blog.csdn.net/vboy1010/article/details/7868645](http://blog.csdn.net/vboy1010/article/details/7868645)

[http://blog.csdn.net/vboy1010/article/details/7892120](http://blog.csdn.net/vboy1010/article/details/7892120)

[http://blog.zoomquiet.org/pyblosxom/oss/openresty-intro-2012-03-06-01-13.html](http://blog.zoomquiet.org/pyblosxom/oss/openresty-intro-2012-03-06-01-13.html)

[http://huoding.com/2012/08/31/156](http://huoding.com/2012/08/31/156)