---
title: 准备工作
---

## 安装Node.js

第一步我们要安装的是Node.js。

Node.js是一个Javascript的运行环境，简单的来说：他把Javascript的引擎从浏览器中分离出来，使得我们在脱离浏览器的时候也能利用Javascript做一些事情。

下载地址为[https://nodejs.org/en/](https://nodejs.org/en/)

![](./assets/1_1.jpg)

官网上目前有两个版本，一个是LTS版，另外一个是Current版，所谓的LTS，就是长期维护版，所以我们下载这个LTS版本就好了。

下载并安装完成之后，打开终端（对于Linux或macOS用户就是terminal，对于Windows用户就是cmd或powershell，我用的是powershell），执行如下命令。

```
λ node -v
v6.10.0

λ npm -v
3.10.10
```

可以得到当前的Node.js的版本号和npm的版本号，如果能正确获得版本号，则意味着安装成功。

## NPM换源

如果你有Linux的使用经验的话，就知道包管理器一般会有多个镜像站，来提升不同地区的访问速度，那么npm作为一款面向Node.js的包管理器，也有换源的方式，所以如果你是中国大陆的用户的话，我强烈建议你换源到中国本地服务器，如果你断定自己不需要换源，请跳过这一部分的阅读。

首先，安装`nrm`。

```
npm install nrm -g
```

安装完毕以后，可以通过以下命令来浏览目前有的源。

```
λ nrm ls

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

可以看到列出了若干个源，`nrm ls`命令会在当前使用的源前方加\*号来提示用户，其中`cnpm`和`taobao`两个源为中国人提供的源。

接下来我们使用`nrm use taobao`就可以切换到taobao源了，如果速度依旧不佳你也可以尝试使用cnpm。

```
λ nrm use taobao

   Registry has been set to: https://registry.npm.taobao.org/
```
