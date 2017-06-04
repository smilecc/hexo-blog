---
title: 1.4 Vue实例
---

那么我们来深入了解一下我们第一个App的代码吧。

## 模块导入

```js
import Vue from 'vue'
```

这是ES2015的语法，是将包导入到我们的项目中，但是主流的浏览器并不支持这种语法，那么他是怎么生效的呢？

还记得我们安装的`babel-loader`吗？就是它帮助我们把`import`转换为浏览器可以执行的JS的。

在我们以后的项目中还将多次使用`import`来导入其他的包。

## 构造器

接下来我们使用Vue提供的**构造函数**，实例化了一个Vue的根实例。

```js
const app = new Vue({
    // 选项
})
```

在实例化 Vue 时，需要传入一个**选项对象**，它可以包含数据、模板、挂载元素、方法、生命周期钩子等选项。全部的选项可以在 [API 文档](http://cn.vuejs.org/v2/api/)中查看。

于是在我们的Hello world例子中传递的选项是这样的：

```js
{
    el: '#app',
    data () {
        return {
            msg: 'hello world'
        }
    }
}
```

这里的`el`意为element，需要提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。

而`data`就是我们存储数据的对象。

## 属性与方法

Vue不同于传统的DOM模型操作方式，所有的数据和方法都被包含在构造时传入的选项对象中，而DOM的传统事件则**完全由Vue接管**，所以在Vue中使用jQuery等DOM封装库是不可取的，这不仅会曾加整个项目文件的大小，还会扰乱开发的思路。

每个Vue实例会**代理**其`data`对象中的所有属性，当这些属性被代理之后，属性会变为**响应的**，响应的意思就是说，当属性被修改之后，页面上所引用的属性也会跟随着变动，我们用一个例子来说明这个现象。

**HTML**

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>随机数</title>
</head>
<body>
    <div id="app">
        <div>{{ random }}</div>
        <button v-on:click="makeRandom">取随机数</button>
    </div>
    <script type="text/javascript" src="main.out.js"></script>
</body>
</html>
```

**Javascript**

```js
import Vue from 'vue'

const app = new Vue({
    el: '#app',
    data () {
        return {
            random: 0
        }
    },
    methods: {
        makeRandom () {
            this.random = Math.round(Math.random() * 100);
        }
    }
});
```

这是一个很简单的随机数生成，页面上的“取随机数”按钮每点一次，页面上的数字就是重新随机一个。

我们可以看到我们使用`v-on:click`属性给这个按钮的onclick事件绑定了一个函数，而这个函数就是`methods`属性中的`makeRandom`函数，这种带有`v-`前缀的特殊属性，在Vue.js中被称之为**指令**。

可以看到这个函数就一句话，给`this.random`赋值，赋的值为一个随机数乘以100然后取整。

这个例子可以说明很多事情：

* 利用**双大括号**可以向页面中插值
* `data`对象中的被代理的属性为**动态**的
* `this`指向的是当前的实例对象
* `data`对象在方法中使用`this`就可以访问的到
* 方法（函数）写在`methods`对象之中
* 通过`v-on`可以监听事件，在这里我们监听的是`click`事件

但请**注意**，只有被代理的属性才为动态的，构造时不在`data`对象中，在实例创建之后才添加新到实例上的属性，它将**不会触发视图更新**。

## 生命周期

每个 Vue 实例在被创建之前都要经过一系列的初始化过程，在每个过程的开始或结束的时候，我们可以利用**生命周期钩子**，来实现我们自己的逻辑。

所谓钩子（Hook），听起来有点抽象，简单的来说就是**一种事件机制**，在**某个时间点框架会自动调用**我们开发者所编写的函数，这样我们就能参与到整个生命周期之中去，比如我们可以决定在实例初始化的时候干什么，在实例销毁的时候干什么，钩子就是起到这个作用。

比如如下的`created`钩子，就将会在实例创建之后被调用。

```js
const app = new Vue({
    data: {
        a: 1
    },
    created: function() {
        // `this` 指向 vm 实例
        console.log('a is: ' + this.a)
    }
});
```

下图所展示的是Vue实例的**整个生命周期**，所有被**红色带有虚线的箭头指着的红色方框**中的内容，就是我们**所有**可以利用的钩子。

看不懂不要紧，你只需知道当需要的时候可以查阅这张图示就可以了。![](./assets/1_4.png)

