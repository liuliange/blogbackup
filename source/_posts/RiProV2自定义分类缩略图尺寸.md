---
title: RiProV2自定义分类缩略图尺寸
top: 0
categories:
  - null
  - null
essayending: true
declare: true
valineenbale: true
toc: false
reward: true
abbrlink: d17dfafe
date: 2022-10-02 15:16:33
tags: [随笔]
---
WordPress原本是自带分类缩略图尺寸自定义设置功能的，但是油条把RiPro-V2主题的自定义功能给屏蔽了，今天我们就来叫大家如何开启分类自定义缩略图尺寸，这样我们就可以自定义没一个分类实现不一样的尺寸了。<!-- more -->


开启方法
修改：ripro-v2\ripro-v2\inc\options\taxonomy-options.php将注释去掉。原始如下：
```
        array(
            'id'      => 'is_no_archive_filter',
            'type'    => 'switcher',
            'title'   => '关闭筛选',
            'label'   => '关闭当前类目下的高级筛选功能',
            'default' => false,
        ),

        // array(
        //     'id'      => 'is_thumb_px',
        //     'type'    => 'switcher',
        //     'title'   => '自定义分类下文章缩略图宽高',
        //     'label'   => '因前台是自适应布局，具体宽高比例前台刷新观察，这里的宽高是图片裁剪真实宽高,在纯分类页面和首页单独分类模块有效',
        //     'default' => false,
        // ),
        // array(
        //     'id'         => 'thumb_px',
        //     'type'       => 'dimensions',
        //     'title'      => '缩略图宽高',
        //     'default'    => array(
        //         'width'  => '300',
        //         'height' => '200',
        //         'unit'   => 'px',
        //     ),
        //     'dependency' => array('is_thumb_px', '==', 'true'),
        // ),
        
        array(
            'id'          => 'archive_single_style',
            'type'        => 'select',
            'title'       => '侧边栏',
            'placeholder' => '',
            'options'     => array(
                'none'  => '无',
                'right' => '右侧',
                'left'  => '左侧',
            ),
            'default'     => _cao('archive_single_style'),
        ),
```
```
        array(
            'id'      => 'is_no_archive_filter',
            'type'    => 'switcher',
            'title'   => '关闭筛选',
            'label'   => '关闭当前类目下的高级筛选功能',
            'default' => false,
        ),

         array(
             'id'      => 'is_thumb_px',
             'type'    => 'switcher',
             'title'   => '自定义分类下文章缩略图宽高',
             'label'   => '因前台是自适应布局，具体宽高比例前台刷新观察，这里的宽高是图片裁剪真实宽高,在纯分类页面和首页单独分类模块有效',
             'default' => false,
         ),
         array(
             'id'         => 'thumb_px',
             'type'       => 'dimensions',
             'title'      => '缩略图宽高',
             'default'    => array(
                 'width'  => '300',
                 'height' => '200',
                 'unit'   => 'px',
             ),
             'dependency' => array('is_thumb_px', '==', 'true'),
         ),
        
        array(
            'id'          => 'archive_single_style',
            'type'        => 'select',
            'title'       => '侧边栏',
            'placeholder' => '',
            'options'     => array(
                'none'  => '无',
                'right' => '右侧',
                'left'  => '左侧',
            ),
            'default'     => _cao('archive_single_style'),
        ),
```
打开/wp-content/themes/ripro-v2/inc/template-tags.php
搜索：根据模式输出缩略图img 延迟加载html标签，将下边代码：
```
if (!function_exists('_get_post_media')) {
    function _get_post_media($post = null, $size = 'thumbnail',$video = true) {
        if (empty($post)) {
            global $post;
        }elseif (is_numeric($post)) {
            $post = get_post($post);
        }

        $_size_px = _get_post_thumbnail_size();

        $src = _get_post_thumbnail_url($post, $size);
```
替换成即可。
```
if (!function_exists('_get_post_media')) {
    function _get_post_media($post = null, $size = 'thumbnail',$video = true) {
        if (empty($post)) {
            global $post;
        }
        $category = get_the_category($post->ID);
        $catid = $category[0]->term_id;
        if (get_term_meta($catid, 'is_thumb_px', true)) {
            $_size_px = get_term_meta($catid, 'thumb_px', true); //缩略图高度
        }else{
            $_size_px = _get_post_thumbnail_size();
        }

        $src = _get_post_thumbnail_url($post, $size);
```
更改后台分类多了一个选项：

![](https://gitee.com/musangking/blogImage/raw/master/blogImage/202210021522149.png)