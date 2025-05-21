---
title: 创建随机图片API
date: 2025-05-16 19:15:27
# updated:
tags:
    - 网站
    - PHP
categories: 技术文档
# keywords:
# description:
top_img: transparent
# comments:
# cover:
# toc:
# toc_number:
# toc_style_simple:
# copyright:
# copyright_author:
# copyright_author_href:
# copyright_url:
# copyright_info:
# mathjax:
# katex:
# aplayer:
# highlight_shrink:
# aside:
# abcjs:
# noticeOutdate:
---

## 1. 创建站点

在宝塔面板的网站中添加站点作为API地址，将网站开启PHP，并申请一个SSL认证并打开强制https

## 2. 创建图片链接

打开这个站点的目录，创建一个`img.txt`和一个`index.php`

在PicList的管理中中全部选中并复制图片url，将链接粘贴到img.txt中

在`index.php`写入如下内容

```php
<?php
//存有image链接的文件名img.txt
$filename = "img.txt";
if(!file_exists($filename)){
    die('文件不存在');
}
 
//从文本获取链接
$pics = [];
$fs = fopen($filename, "r");
while(!feof($fs)){
    $line=trim(fgets($fs));
    if($line!=''){
        array_push($pics, $line);
    }
}
 
//从数组随机获取链接
$pic = $pics[array_rand($pics)];
 
//返回指定格式
$type=$_GET['type'];
switch($type){
 
//JSON返回
case 'json':
    header('Content-type:text/json');
    die(json_encode(['pic'=>$pic]));
 
default:
    die(header("Location: $pic"));
}
?>
```

---
参考文章

[创建自己的随机图片api](https://www.oldming.cn/archives/chuang-jian-zi-ji-de-sui-ji-tu-pian-api)
