---
title: '转：php源码随机输出某个目录下的图片API'
categories:
  - 学编程
tags:
  - 混技能
toc: true
comments: true
keywords: ''
description: '搞了一大批美女图，想给网友过过眼，写个php源码随机输出某一个目录下的图片，可用于随机图片API接口。'
date: 2020-09-19 23:10:23
updated: 2020-09-19 23:10:23
top:
---
# 前言
搞了一大批美女图，想给网友过过眼，写个php源码随机输出某一个目录下的图片，可用于随机图片API接口。

# 代码
把下面的代码保存为 `imgapi.php` ：
```
<?php
header('Content-type: image/jpg');
$img_array = glob("./*.{gif,jpg,png}",GLOB_BRACE);
$img = array_rand($img_array);
$image = file_get_contents($img_array[$img]);
echo $image;
?>
```
保存就可以调用这个接口。

# 使用方法

调用方式为：`域名/imgapi.php`，图片目录为 `imaapi.php` 同级目录。

也可以自定义图片的目录，把下面代码：
```
$img_array = glob("./*.{gif,jpg,png}",GLOB_BRACE);
```

改为以下代码，`img` 是图片目录名，可以随意更改：
```
$img_array = glob("./img/*.{gif,jpg,png}",GLOB_BRACE);
```
这样就可以调用 `img` 目录下的图片了。

[原文链接](https://www.vmitu.com/webdaima/2020032051.html)