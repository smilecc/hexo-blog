---
title: 1.2 Webpack入门
---

在我们简单的学习过NPM如何使用之后呢，接下来我们要学习一个对Vue开发同样关键的工具——webpack。

事实上我们可以通过使用官方提供的`vue-cli`工具来跳过这一部分的手动配置，但是由于这部分很重要，手动配置一遍有利于我们理解webpack的配置文件的配置方法和webpack到底起什么作用。

我思考过如果先跳过webpack的学习，直接开始Vue的学习可不可行，但我仔细考虑之后还是否定了这个想法，我知道大家迫不及待的想要进入正篇的学习，但是这里的铺垫却是不可缺少的。（喂喂，废话适可而止啊笨蛋）

## 安装

webpack像是nrm一样，是一个我们长期要使用的工具，所以我们选择全局安装。

```
npm install webpack@2.2.1 -g
```

可以看到，和之前的install又有了一点不同，这次是在我们要安装的包名后面加上了`@2.2.1`，聪明的朋友可能已经明白了，这就是指定版本号。

经过我之前的测试，如果不带版本号可能会被安装webpack1，而2是不兼容1的配置文件的语法的，所以我们要指定安装webpack2。

安装完毕以后，惯例，我们查看一下webpack的版本号，成功执行则意为安装成功。

```
λ webpack -v
2.2.1
```

安装完毕之后，由于我们的项目依赖于webpack，所以我们要把webpack加入到我们的package.json中，不用手动修改，直接再install一遍。

```
npm install webpack@2.2.1 --save
```

---

## 配置

我们在项目根目录下建立一个名为webpack.config.js的js文件，顾名思义，这是webpack的配置文件。

当配置文件名为webpack.config.js的时候，webpack会自动加载这个文件，如果想叫其他名字，也可以手动指定，详细内容可以参见webpack的[文档](https://webpack.js.org/configuration/)，这里就不赘述了。

将如下配置内容保存到刚才建立的配置文件中。

```js
var path = require('path');
var webpack = require('webpack');

module.exports = {
    entry: __dirname + "/main.js", //唯一入口文件
    output: {
        path: __dirname, //打包后的文件存放的地方
        filename: "main.out.js" //打包后输出文件的文件名
    },
    module: {
        // 规则匹配
        rules: [
        {
            test: /\.js$/, // 匹配什么文件
            loader: 'babel-loader', // 用什么加载器来解析
            exclude: /node_modules/ // 排除node_modules文件夹
        }]
    },
    // 解释参见 http://cn.vuejs.org/v2/guide/installation.html#独立构建-vs-运行时构建
    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.common.js'
        }
    },
    devServer: {
        historyApiFallback: true,
        noInfo: true
    },
    performance: {
        hints: false
    },
    devtool: '#eval-source-map'
}
```

在配置文件中，有以下几个至关重要的部分：

* entry（入口文件）
* output（输出文件）
* module -&gt; rules（loader的规则）

至于其他的部分，可以参见webpack文档（英文的）。

### 入口文件

首先我们要建立我们的入口文件，我这里建立了一个名叫main.js的js文件。

`__dirname`这个变量存储的是我们当前配置文件的路径，所以`__dirname + '/main.js'`意为使用当前配置文件路径下的main.js作为入口文件。

有人会问，当初咱们在`npm init`初始化项目的时候入口文件不是默认的index.js吗，要不要去修改？

答案是不用，咱们用不到那个入口文件，这里以webpack中的入口文件为准。

### 输出文件

无需多说，填写webpack打包过后代码的存放的路径和文件名。

### Rules

这一部分至关重要，为了咱们第一个建立的配置文件大家都能看懂，并足够整洁，我省去了其他的一些loader，只保留了处理js文件的这个loader。

所谓loader，就是加载器，为什么要有loader呢，例如我们想使用ES2015语法，但主流浏览器却不支持，我们就想办法通过一个预处理机制，把我们的代码通过一个loader解析成正确的内容。

类似的还有用来加载scss的loader、用于加载图片文件的file-loader，以及我们伴随我们日后的vue-loader。

我们这里使用了babel-loader来加载js文件，所以我们要在npm中安装这个loader。

```
npm install babel-loader --save-dev
```

当我们安装完毕会注意到下方会多出一行WARN（warring）

```
npm WARN babel-loader@6.3.2 requires a peer of babel-core@^6.0.0 but none was installed.
```

这是告诉我们babel-loader依赖于babel-core，而我们却没有安装，所以我们来安装他。

```
npm install babel-core --save-dev
```

这里的`--save-dev`指令作用是将这次安装的包保存到`devDependencies`中，你可以打开`package.json`确认一下，会发现多出了一个`devDependencies`，顾名思义就是这些包是只在开发的时候使用，在发布的时候是不需要打包的。

这样我们安装好了这个loader，于是我们终于可以开始开发基于vue的第一个**hello world**了。

