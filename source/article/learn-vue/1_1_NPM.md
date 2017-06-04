---
title: 1.1 NPM入门
---

看到这里，很多朋友可能会有疑问，**老子不是来学Vuejs的吗，怎么第一章是NPM入门呢？**

前面已经说了，咱这个教程是面向新手的，而很多新手根本没有接触过NPM，如果像是官方文档那样，上来就讲Vue自身，而忽略了这些基础知识的话，很多朋友可能会被搞的一头雾水，所以磨刀不误砍柴工，咱们从NPM讲起。

## 初始化一个项目

首先咱们还是打开终端，在你喜欢的位置创立并进入一个文件夹，咱们将在这个文件夹中初始化一个项目。

``` bash
λ mkdir learnvue
λ cd learnvue
λ npm init
```

在执行`npm init`之后，将会出现很多的英文，请不要慌张，我们一点一点来看。
```
    This utility will walk you through creating a package.json file.
    It only covers the most common items, and tries to guess sensible defaults.

    See `npm help json` for definitive documentation on these fields
    and exactly what they do.

    Use `npm install <pkg> --save` afterwards to install a package and
    save it as a dependency in the package.json file.

    Press ^C at any time to quit.
    name: (learnvue)
```
首先上面出现的几段英文，看不懂也没关系，就是告诉我们一些帮助信息。

此时光标将会停留在`name: (learnvue)`之后的位置，意思是让我们按照提示输入这些信息，输入好一个信息之后，敲击回车键确认并进入下一项的填写，让我来解释一下这些内容。

```
name: (learnvue) --- 名称
version: (1.0.0) --- 版本号
description: --- 介绍
entry point: (index.js) --- 入口点（入口文件）
test command: --- 测试命令
git repository: --- git库
keywords: --- 关键词
author: --- 作者
license: (ISC) --- 协议
```

而有些项目的冒号之后会有一个括号，括号里面会有一些内容，这个是告诉我们这一些的默认值，也就是在我们直接敲击回车的情况下，这一项的值，如果没有括号，就意为不填就为空。

所以关键的属性都有默认值了，你可以认真填写，也可以一路回车，都是不影响的，而且之后也可以改。

在填写完最后一项（license）之后，会按照我们的填写展示我们要生成的配置文件的内容。

```bash
About to write to G:\Code\nodejs\learnvue\package.json:

{
  "name": "learnvue",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes) yes
```

最后他询问我们`Is this ok?`是否OK，确认则直接回车或输入yes，反之则输入其他的内容就可以取消。

这时，我们可以看到我们的文件夹下会多出一个叫`package.json`的文件，这个文件就是根据我们填写的信息生成的配置文件。

---

## 安装Vue

接着我们就使用npm给我们的项目安装vue，之前我们使用过`npm install nrm -g`来给npm安装源管理器，类似的，我们也是使用`npm install`来给一个项目安装包。

首先我们安装的是Vue。
```bash
    λ npm install vue --save
    learnvue@1.0.0 G:\Code\nodejs\learnvue
    `-- vue@2.2.1
```
安装完毕之后，你会发现你的项目文件夹中多出了一个叫`node_modules`的文件夹，这个文件夹就是用来存储包的文件的。

之前我们安装nrm的时候使用了一个`-g`的指令，这个指令实际上的意思是global（全局的）。

而这里我们使用了一个`--save`的指令，是因为如果直接执行`npm install vue`的话，这样安装的包将仅仅出现在`node_modules`文件夹中，而不会出现在我们的package.json中。

将使用的包名写入package.json文件的意义是，当我们要备份、迁移项目的时候，我们不需要连带庞大的`node_modules`文件夹拷贝，而是只需要将package.json拷贝走，在新的项目目录中重新执行`npm install`就可以直接从包管理器中拉取我们项目依赖的包了。

