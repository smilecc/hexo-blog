---
title: ThinkPHP在低版本PHP下引用外部库的一个Bug
id: 570
categories:
  - 网站开发
date: 2016-04-27 22:20:27
tags:
  - PHP
---

## 外部库文件引用

首先在库引用的时候，在写required的时候要相对于index.php，也就是入口文件。

比如我要在`./Application/Home/Controller/IndexCotroller.class.php`中引用`./Application/Common/Common/Library.php`的时候

不能写`../../../Common/Common/Library.php`，而必须写相对于入口文件的路径 `./Application/Home/Controller/IndexCotroller.class.php`

## 一个Bug

我此时使用的ThinkPHP版本是3.2.3，经过测试发现在PHP 5.3和5.4低版本的时候，在不开启Debug模式的情况下，也就是部署模式下。

会出现找不到`/Think/Org`命名空间的情况，这个Bug应该是PHP的Bug而不是ThinkPHP的Bug。

进过查阅资料，发现在`./ThinkPHP/Library/Think/Think.class.php`的38行也就是`Storage::connect(STORAGE_TYPE);`

后添加`version_compare(PHP_VERSION, '5.4.21', '&lt;') &amp;&amp; Log::INFO;`

然后删除缓存，就可以解决这个问题。