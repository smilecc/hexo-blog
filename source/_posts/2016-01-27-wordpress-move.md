---
title: 再一次的博客迁移&&写了一个域名置换处理脚本
id: 497
categories:
  - 网站开发
date: 2016-01-27 23:43:36
tags:
  - PHP
---

又把自己的博客迁移到了位于Hostker的VPS上，之前用的是恒创的PHP空间，在香港，不过发现学校机房的电信基本上无法访问，还是选择迁移出。

按照一般思路，我把网站的物理文件压缩后下载到本地，然后传输到我的VPS上使用Unzip解压。

关于数据的迁移也相对简单，直接把原有的数据以Sql文本的形式导出，然后到新的服务器上执行即可。
<!-- more -->
Wordpress 关于数据库的配置都在 wp-config.php 这个文件里，直接vim改掉就好。

不过由于我在本次迁移当中，将原有的cuican.name改用为blog.cuican.name，所以导致了图片失效的问题。

我使用的是Dnspod，通过Dnspod提供的URL显示功能提供的302跳转，确实可以使得图片的正常显示，不过如果以后弃用该功能，重新使用A记录解析，应该还是要做处理，不如一次性搞定，于是写了如下的一个简单的PHP脚本来置换掉整站所有文章和页面内的域名。

原理也很简单，把所有的数据取出来，然后替换文本，再更新回去，使用PDO的原因也是PDO支持预处理查询，免去了拼接字符串处理字符串内SQL关键词等一系列的麻烦。

### 请注意，如果你使用这个脚本：

**1\.  **你会发言一篇文章出现多次

[![497-1](http://blog.cuican.name/wp-content/uploads/2016/01/497-1.png)](http://blog.cuican.name/wp-content/uploads/2016/01/497-1.png)

如图，这是因为WP会把你的历史版本也存储下来，所以就会出现这个情况，请不用担心，不是Bug。

**2.** 仅执行一次即可，如果像我这样的一级域名改二级域名，多次执行会出现最终被替换为了blog.blog.blog.cuican.name的现象。

&nbsp;

### 脚本代码

``` php
<?php
ini_set("display_errors", "On");
header("Content-type: text/html; charset=utf-8");

// 改为你的Sql连接字符串
$db = new PDO("mysql:host=localhost;dbname=test","root","root");
$db->exec("SET NAMES 'utf8';");
$rs = $db->query("SELECT * FROM wp_posts");
$articleList = $rs->fetchAll();

$stmt = $db->prepare("UPDATE wp_posts SET post_content=:content WHERE ID=:id");
$stmt->bindParam(':content', $sql_content);
$stmt->bindParam(':id', $sql_id);

foreach ($articleList as $key => $value) {
	$content = $value['post_content'];

	$content_replace = str_replace(
			array('cuican.name','smilecc.sinaapp.com'), // 改为你的原始域名，如果仅一个域名可以直接将数组改为字符串
			"blog.cuican.name", // 改为你现在的域名
			$content, 
			$count);

	if($count > 0)
	{
		$sql_content = $content_replace;
		$sql_id = $value['ID'];
		$up_count = $stmt->execute();

		if($up_count > 0)
		{
			echo '命中文章 -> ID:'.$value['ID'].' - 【'. $value['post_title'] .'】 共计：'. $count."\r\n";
		}
		else
		{
			echo '命中失败 -> ID:'.$value['ID'].' - 【'. $value['post_title'] .'】 共计：'. $count."\r\n"; 
		}
	}
	
}
```