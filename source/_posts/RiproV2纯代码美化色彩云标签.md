---
title: RiproV2纯代码美化色彩云标签
top: 0
categories:
  - Ritheme
  - Wordpress
essayending: true
declare: true
valineenbale: true
toc: false
reward: true
abbrlink: 7b65b034
date: 2022-10-28 14:13:27
tags: [随笔]
---
WordPress 自带的标签云是一个很实用的小工具。站长可以通过标签对具有相同关健词的文章进行检索分类，利于访客查找相关文章。博主使用的​​RiproV2主题​​​的标签云虽然有颜色，但是也不是彩色的。所以想到了自定义代码实现​​圆角彩色标签云​​。 <!-- more -->

使用方法：在当前主题目录下面的​​functions.php​​里面加入以下代码：

```
/***圆角背景色标签***/
function colorCloud($text) {  
$text = preg_replace_callback('|<a (.+?)>|i', 'colorCloudCallback', $text);  
return $text;  
}  
function colorCloudCallback($matches) {  
$text = $matches[1];  
$colors = array('48D1CC','a26ff9','fb7da9','666','19B5FE','ff5e5c','ffbb50','1ac756');  
$color=$colors[dechex(rand(0,7))]; 
$pattern = '/style=(\'|\")(.*)(\'|\")/i';  
$text = preg_replace($pattern, "style=\"display: inline-block; *display: inline; *zoom: 1; color: #fff; padding: 1px 3px; margin: 0 3px 3px 0; background-color: #{$color}; border-radius: 3px; -webkit-transition: background-color .4s linear; -moz-transition: background-color .4s linear; transition: background-color .4s linear;\"", $text);  
$pattern = '/style=(\'|\")(.*)(\'|\")/i';  
return "<a $text>";  
}  
add_filter('wp_tag_cloud', 'colorCloud', 1);

```
然后到网站后台，依次点击​​外观​​——​​小工具​​——​​标签云​​，添加以后就可以看到圆角彩色标签云了。

默认的就已经非常漂亮了，如果不喜欢也可以自己对照​​rgb颜色表​​修改：
```colors = array('48D1CC','a26ff9','fb7da9','666','19B5FE','ff5e5c','ffbb50','1ac756');  ```


效果如下图：
![](https://cdn.jsdelivr.net/gh/liuliange/blogPicture/blogPicture/202210281428248.png)

你的标签云就变彩色了，是吧！！！