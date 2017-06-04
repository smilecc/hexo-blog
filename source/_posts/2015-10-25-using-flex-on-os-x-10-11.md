---
title: Using Flex On OS X 10.11
id: 480
categories:
  - 工具
date: 2015-10-25 12:18:39
tags:
---

在OS X上直接使用Flex会出现一些问题，首先是标准的问题，这边会出两个Warring，这个是采用C99标准导致的。

毕竟Flex是个老玩意了。
<!-- more -->
[![36F3EE81-A2EC-4E04-BACB-C6FF2F78DECF](http://blog.cuican.name/wp-content/uploads/2015/10/36F3EE81-A2EC-4E04-BACB-C6FF2F78DECF.jpg)](http://blog.cuican.name/wp-content/uploads/2015/10/36F3EE81-A2EC-4E04-BACB-C6FF2F78DECF.jpg)

这边只要加上 -std=gnu89，也就是采用GNU的C89标准。

[![屏幕快照 2015-10-25 下午12.16.13](http://blog.cuican.name/wp-content/uploads/2015/10/屏幕快照-2015-10-25-下午12.16.13.png)](http://blog.cuican.name/wp-content/uploads/2015/10/屏幕快照-2015-10-25-下午12.16.13.png)

然后又会出个新问题，就是库的丢失。

再加上-ll即可。

[![F3BFAB3A-964B-4BD1-83E9-7DF1B1690FD4](http://blog.cuican.name/wp-content/uploads/2015/10/F3BFAB3A-964B-4BD1-83E9-7DF1B1690FD4.jpg)](http://blog.cuican.name/wp-content/uploads/2015/10/F3BFAB3A-964B-4BD1-83E9-7DF1B1690FD4.jpg)