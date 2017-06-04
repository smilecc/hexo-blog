---
title: 在Ubuntu 14.04 上安装并配置饥荒联机版服务器(Don’t Starve Together Server)
id: 517
categories:
  - 游戏
date: 2016-02-11 19:31:36
tags:
---

**本文由璨翻译自：[来源链接](https://www.linode.com/docs/applications/game-servers/dont-starve-together-on-ubuntu)**

Don’t Starve Together 是一个多人在线游戏，由Klei公司编写并发布，这个游戏原本是一个单人游戏，所谓多人版（Together版）其实就是在原本单人游戏的基础上添加了多人模式。

这篇文章将会教你如何准备你的VPS、安装SteamCMD和安装与配置饥荒联机版（Don’t Starve Together）。

注：下文将把Don’t Starve Together简称为DST。
<!-- more -->
# 准备

你需要在开始前准备这些东西：

*   一个 Steam 账号
*   在你的 Steam中 一份已经购买的DST
*   一个已经在运行的 Ubuntu 14.04 服务器
> Note
> 
> 
> 这份手册是为非root用户编写的，在命令前使用 sudo 的目的是为了提高权限，如果你不熟悉 sudo 命令，建议你谨慎使用或参考相关文档。

* * *

# 服务器准备

DST 在 Steam上出售，所以我们需要一份有效的游戏授权。因此我们首先需要使用 SteamCMD 来下载游戏，使之为 Steam 上的该游戏提供在线服务器。

本步骤将准备服务器所需的依赖环境。

由于现在使用的服务器大部分都是 64位 操作系统，所以我们需要为 SteamCMD 以及 DST 安装一些外部依赖库。

### 1\. 向包管理器中添加i386的构建器

``` bash
sudo dpkg --add-architecture i386
```

### 2\. 更新包管理器资源

```
sudo apt-get update &amp;&amp; sudo apt-get upgrade
```

### 3\. 安装 32位依赖库

``` bash
sudo apt-get install lib32gcc1 lib32stdc++6 libcurl4-gnutls-dev:i386 screen
```
> Note
> 
> 
> 如果你运行的是32位系统，请使用
> 
> <pre class="theme:github lang:default decode:true" style="padding-left: 30px;">sudo apt-get install libcurl4-gnutls-dev:i386 libgcc1 screen</pre>

* * *

# 安装 SteamCMD 和 DST

### 1\. 在你的Home文件夹中，下载 SteamCMD

``` bash
mkdir steamcmd
cd steamcmd
wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
```

### 2\. 解压

``` bash
tar -xvzf steamcmd_linux.tar.gz
rm steamcmd_linux.tar.gz
```

### 3\. 运行 SteamCMD 安装器
``` bash
./steamcmd.sh
```
这个命令在执行时将会输出类似的如下信息
``` bash
Redirecting stderr to '/home/linode/Steam/logs/stderr.txt'
[  0%] Checking for available updates...
[----] Downloading update (0 of 7,013 KB)...
[  0%] Downloading update (1,300 of 7,013 KB)...
[ 18%] Downloading update (3,412 of 7,013 KB)...
[ 48%] Downloading update (5,131 of 7,013 KB)...
[ 73%] Downloading update (6,397 of 7,013 KB)...
[ 91%] Downloading update (7,013 of 7,013 KB)...
[100%] Download complete.
[----] Installing update...
[----] Extracting package...
[----] Extracting package...
[----] Extracting package...
[----] Installing update...
[----] Installing update...
[----] Installing update...
[----] Cleaning up...
[----] Update complete, launching Steam...
Redirecting stderr to '/home/linode/Steam/logs/stderr.txt'
[  0%] Checking for available updates...
[----] Verifying installation...
Steam Console Client (c) Valve Corporation
-- type 'quit' to exit --
Loading Steam API...OK.

Steam>
```
这个 Steam> 类似于Linux的命令提示符，在显示这个的时候不能执行一般Linux命令。

### 4\. 从 SteamCMD 安装 DST

``` bash
login anonymous
force_install_dir ../dstserver
app_update 343050 validate
```
这可能需要一定的时间，你需要有耐心的等待，大概十分钟，你会按到这样的输出：
``` bash
Success! App '343050' fully installed.

Steam>
```

### 5\. 结束，退出 SteamCMD
``` bash
quit
```

* * *

# 配置 DST 服务器

1\. 在配置你的DST之前，你需要先启动一次来生成配置文件
``` bash
cd ~/dstserver/bin
./dontstarve_dedicated_server_nullrenderer
```
2\. 当你看到如下输出，说明这个服务器被成功启动了
``` bash
Telling Client our new session identifier: XXXXXXXXXXXXXXXX
ModIndex: Load sequence finished successfully.  
Reset() returning
```
不过你会看到这样的错误
``` bash
[200] Account Failed (6): "E_INVALID_TOKEN"
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!! Your Server Will Not Start !!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
```
这是正常现象，我们将在下个步骤中修复它。

3\. 使用 **Control + C **退出服务器，你会返回到Linux操作环境中。

4\. 所被创建的 DST 文件被保存在 ~/.klei/DoNotStarveTogether/ 中，所生成出来的配置文件仅仅是一个模板文件，你需要自己把它修改成你需要的配置文件，注意那些非单一选项，就是使用 | 分割的那些，和那些数字范围的选项（中间用 ... 标识的），请把它改成单一的，才可以使用。

##### 生成出来的模板文件 ：

<dl class="file"><dt>~/.klei/DoNotStarveTogether/settings.ini</dt></dl>
``` bash
[network]
default_server_name = Your unique server name
default_server_description = A very nice server description
server_port = 10999
server_password = password
max_players = 1 .. 64
pvp = true | false
game_mode = endless | survival | wilderness
 enable_autosaver = true | false
tick_rate = 30
connection_timeout = 8000
server_save_slot = 1
enable_vote_kick = true | false
pause_when_empty = true | false

[account]
dedicated_lan_server = false

[STEAM]
DISABLECLOUD = true

[MISC]
CONSOLE_ENABLED = true
autocompiler_enabled = true
```
5\. 创建启动脚本 start_dst.sh，将如下内容填写进去
``` bash
screen -S "DST Server" dontstarve_dedicated_server_nullrenderer
```

* * *

# 获取你的认证令牌(Authentication Token)

服务器管理员（GSAs）必须拥有饥荒联机帐号才能架设线上独立服务器，单一帐号即可架设多个独立服务器。

所以为了运行公共的服务器，GSA需要一个”server_token”文件，这个文件提供了饥荒的所有权，并且允许Klei公司改善那些违反了条款的服务器。

这一步需要在你本地的PC中进行，所以请打开你本地Steam中的DST。

1\.  打开你的游戏，点进入游戏（Play！）进入到主菜单当中，按`键（Tab上面的那个键）打开控制台，你会看到如下画面

![控制台](http://blog.cuican.name/wp-content/uploads/2016/02/2.png)

> 如果你从未玩过这个游戏，你则需要点Play!来创建一个账户

2\. 在控制台中运行如下命令

``` C#
TheNet:GenerateServerToken()
```
将这个命令输入（复制也行）到下方的方框内，按回车（Enter）键运行。当你按下回车，这个控制台会消失，再次按下`可以开启。然后你可以退出游戏，随后在本地的的文件夹中找到生成的文件，这一点根据你的操作系统的不同而有所不同：

在Windos上
```
%USERPROFILE%/My Documents/Klei/DoNotStarveTogether/server_token.txt
```
在Linux上
```
~/.klei/DoNotStarveTogether/server_token.txt
```
在OS X上
```
~/Documents/Klei/DoNotStarveTogether/server_token.txt
```
这个文件就是你的server_token（服务器令牌），不要把它分享给任何人噢o(*≧▽≦)ツ

4\. 把这个文件上传到settings.ini（刚才生成出来的服务器配置文件）的同级目录下，此部分完成~

* * *

# 使用服务器

到这一步基本大功告成 o(*≧▽≦)ツ

1\. 现在你的服务器已经安装和配置好了，现在你可以运行刚才我们安置的 start_dst.sh 脚本了，这个脚本被我们刚才放在 ~/dstserver/bin/ 下了，如果你现在所在的目录不是这个目录，那么使用如下命令到达目录，并运行脚本。
``` bash
cd ~/dstserver/bin/
./start_dst.sh
```
> 请记住！如果你想退出服务器，请使用 **Control + C **(´・ω・`)
2\. 分离当前回话使它运行在服务器端，则使用如下按键

```
Control + A
Control + D
```

3\. 使用如下命令可以恢复回话
``` bash
screen -r
```
4\. 如果你想停止服务器，记得先恢复回话再用 **Control + C。**

* * *

# 进入服务器

现在你已经安装和配置好了你的DST服务器，你拥有了一个完全为你所有的DST服务器，你可以叫上你的小伙伴们一起玩耍，用户可以从服务器列表（Server List）中发现并连接你的服务器，点击连接（Connect），输入密码（如果你设置了的话），就可以加入你的服务器了 (*￣∇￣*)

**Enjoy it!**

本文译自：https://www.linode.com/docs/applications/game-servers/dont-starve-together-on-ubuntu

**译者：璨**、原文链接：https://blog.smilec.org/2016/02/11/2014-11-02-ubuntu-dst-server/

部分内容有删改，转载请注明，谢谢。