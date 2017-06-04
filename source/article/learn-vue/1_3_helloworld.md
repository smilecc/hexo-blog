---
title: 1.3 第一个App
---

终于可以开始写我们的hello world了。

# 开始

建立一个`index.html`和一个`main.js`，其中`index.html`作为页面的入口，而`main.js`作为js的入口。

废话不多说，直接将如下的代码添加到对应的文件中，我们等一会再来解释其中的内容。

**index.html：**

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>第一个App</title>
</head>
<body>
    <div id="app">
        {{ msg }}
    </div>
    <script type="text/javascript" src="main.out.js"></script>
</body>
</html>
```

**main.js：**

```js
import Vue from 'vue'

const app = new Vue({
    el: '#app',
    data () {
        return {
            msg: 'hello world'
        }
    }
});
```

在终端中，在**项目目录**下执行`webpack`命令：

```bash
λ webpack

Hash: 76c655b496b5c09bd418
Version: webpack 2.2.1
Time: 805ms
      Asset    Size  Chunks                    Chunk Names
main.out.js  673 kB       0  [emitted]  [big]  main
   [0] ./~/vue/dist/vue.common.js 242 kB {0} [built]
   [1] ./~/process/browser.js 5.3 kB {0} [built]
   [2] (webpack)/buildin/global.js 509 bytes {0} [built]
   [3] ./main.js 115 bytes {0} [built]
```

于是我们的**依赖库和业务代码**都打包到了我们在`webpack.config.js`中指定的`main.out.js`文件中。

这时候大功告成，我们需要一个Web服务器使得我们能看到自己的成果。

## webpack-dev-server

安装webpack-dev-server，这个工具使我们前端开发者拥有一个极度简单的服务器，以助于我们开发，与此同时，他还具有**热更新**功能，可以实时的输出文件。

安装：

```
npm install webpack-dev-server --save-dev
```

安装完毕之后我们直接在当前的项目目录中执行`webpack-dev-server`命令，来开启一个简易服务器。

```bash
λ webpack-dev-server
Project is running at http://localhost:8080/
webpack output is served from /
404s will fallback to /index.html
```

这时候我们只需要按照指示，在浏览器中打开localhost:8080，即可看到我们的成果。

![](./assets/1_3.jpg)

同样`Ctrl+C`可以关闭服务器。

