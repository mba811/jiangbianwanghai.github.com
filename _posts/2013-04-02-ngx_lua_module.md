---
layout: post
title: "centos下让nginx支持ngx_lua_module模块"
description: "ngx_lua_module 是一个nginx的http模块，它把 lua 解析器内嵌到 nginx中，用来解析并执行lua 语言编写的网页后台脚本"
category: nginx
tags: [centos, nginx, ngx_lua_module, lua]
---
{% include JB/setup %}

ngx_lua_module是一个Nginx的http模块它把Lua解析器内嵌到Nginx中，从而可以让用户在Nginx 核心中直接运行Lua语言编写的程序。我们可以选择在 Nginx不同的请求处理阶段插入我们的Lua代码。这些Lua代码既可以直接内联在Nginx配置文件中，也可以单独放置在外部.lua文件里，然后在 Nginx 配置文件中引用.lua文件的路径。