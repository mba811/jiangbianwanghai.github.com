---
layout: post
title: "php实现字符串加密16进制方法"
description: ""
category: 
tags: [字符串加密, php]
---
{% include JB/setup %}

字符串加密和解密是我们在开发系统的时候经常使用的。特别是使用在url当中。现给出一种加密和解密的方案。

比如：

**Y450联想笔记本电脑**

    <?php
    header("Content-type:text/html;charset=utf-8");
    $string = 'Y450联想笔记本电脑';
    $string = iconv("UTF-8", "GBK//IGNORE", $string);
    $string = strToHex($string);
    echo $string;
    //字符串转化为16进制
    function strToHex($string)
    {
        $hex = "";
        for ($i=0; $i<strlen($string); $i++) {
            $hex .= dechex(ord($string[$i]));
        }
        $hex = strtoupper($hex);
        return $hex;
    }

使用上面的函数可以将上面的词加密成

**59343530C1AACFEBB1CABCC7B1BEB5E7C4D4**

    <?php
    header("Content-type:text/html;charset=utf-8");
    $string = '59343530C1AACFEBB1CABCC7B1BEB5E7C4D4';
    $string = hexToStr($string);
    $string = iconv("GBK", "UTF-8//IGNORE", $string);
    echo $string;

    //16进制转化为字符串
    function hexToStr($hex)   
    {
        $string = "";
        for ($i=0; $i<strlen($hex)-1; $i+=2) {
            $string .= chr(hexdec($hex[$i].$hex[$i+1]));
        }
        return $string;
    }

使用上面的函数可以解密刚才加密后的字符串