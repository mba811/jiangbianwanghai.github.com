---
layout: post
title: "让nginx支持path_info功能"
description: ""
category: 
tags: [nginx, path_info, pathinfo]
---
{% include JB/setup %}

PATH_INFO是一个CGI 1.1的标准，经常用来做为传参载体.

比如, 我们可以使用PATH_INFO来代替Rewrite来实现伪静态页面, 另外不少PHP框架也使用PATH_INFO来作为路由载体.

本人使用php开发了一个框架，模式就是使用的PATH_INFO来进行路由。在WAMP下开发的apache默认是支撑PATH_INFO的。因此，开发过程比较顺利。当我将开发完的框架放到centos的LNMP环境是却出错了。原来nginx默认是不支持PATH_INFO功能的。怎么办，程序写好了总不能不用吧。于是，就开始百度啦。

最好的办法是让nginx支撑PATH_INFO功能。既然他木有，就让他模拟吧。一下是nginx配置的完整源代码

    server {
        listen       80;
        server_name  domain.com;
        root   html/domain;
        index  index.html index.htm index.php;
        
        location / {
            if (!-e $request_filename) {
            rewrite  ^/(.*)$  /index.php/$1  last;
                break;
            }
        }

        location ~ .+\.php($|/) {
            fastcgi_index index.php;
            fastcgi_pass 127.0.0.1:9000;
            include fastcgi_params;
            set $path_info "";
            set $real_script_name $fastcgi_script_name;
            if ($uri ~ "^(.+?\.php)(/.+)$") {
                set $real_script_name $1;
                set $path_info $2;
            }
            fastcgi_param SCRIPT_FILENAME $document_root$real_script_name;
            fastcgi_param SCRIPT_NAME $real_script_name;
            fastcgi_param PATH_INFO $path_info;
        }
    }