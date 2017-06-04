---
title: 1.5 模版语法与计算属性
---

# 模版语法与数据绑定

接下来我们开始做一个简易的选色器，来讲解Vuejs的模版语法。

[在线演示（Demo）](https://smilecc.github.io/learn-vue2x/demo/1_4/)

## 开始

talk is cheap，show me the code。

** HTML **

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>RGB</title>
    <style type="text/css">
        .canvas {
            width: 200px;
            height: 60px;
        }
    </style>
</head>
    <div id="app">
        <div v-bind:style="calcColor" class="canvas"></div>

        <div>
            <span>红</span>
            <input type="range" min="0" max="255" step="1" v-model="red" />
            <span>{{ red }}</span>
        </div>
        <div>
            <span>绿</span>
            <input type="range" min="0" max="255" step="1" v-model="green" />
            <span>{{ green }}</span>
        </div>
        <div>
            <span>蓝</span>
            <input type="range" min="0" max="255" step="1" v-model="blue" />
            <span>{{ blue }}</span>
        </div>
        <button @click="getRGB">获取当前RGB</button>
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
            red: 0,
            green: 0,
            blue: 0
        }
    },
    methods: {
        getRGB () {
            alert(
                '红: '+ this.red + 
                '\n绿: ' + this.green + 
                '\n蓝: ' + this.blue
                );
        }
    },
    computed: {
        calcColor () {
            return 'background-color: rgb(' 
                                + this.red + ', ' 
                                + this.green + ', ' 
                                + this.blue + ')';
        }
    }
});
```

首先我们设定了一个宽200像素，高60像素的div来作为我们的画板。紧接着我们使用了`v-bind:style`将该元素的`style`属性绑定到我们的**计算属性**之中。

我们在计算属性中通过拼接字符串的方式拼接出一行设置背景颜色的样式，尽管**这不是最好的做法**，但是现阶段为了例子的讲解，我们暂且先这么做。

随后我们使用了三个Input Range作为数据的来源（Input Range是HTML5中的新特性，早期浏览器无法支持）。随后，我们使用`v-model`对**表单控件**进行了**双向数据绑定**。

---

## 模版语法

### 插值

前面我们已经了解了最基础的通过双大括号的方式进行插值，在这里我们就来详细了解一下几种插值的方式。

---

#### 文本

通过**双大括号**向页面中进行插值，在上个例子中我们已经学习到这种用法，这也是最常见的数据绑定方式。

```html
<span>{{ red }}</span>
```

当绑定的数据对象上的`red`发生改变，插值处也会自动更新。

如果我们想要数据不再更新，也可以使用`v-once`指令进行一次性插值。

```html
<span v-once>{{ red }}</span>
```

此时当数据对象上的`red`改变，插值处也不会改变。

#### HTML

双大括号会将数据解释为纯文本，当我们需要输出HTML的时候，我们就需要使用`v-html`指令。

```html
<div v-html="rawHtml"></div>
```

被插入的文本会被解析为HTML，在其中进行的数据绑定也会被忽略。

但要注意的是：

1. 不要用`v-html`来合成复杂的HTML，Vue的组件将更能胜任这一点
2. 请**只对可信任的内容来源进行HTML插值**，否则非常容易造成XSS攻击（跨站脚本攻击）

#### 属性

在例子中我们已经看到，当我们想给HTML的元素属性绑定值的时候，我们不能使用双大括号进行插值，而是使用`v-bind`指令进行插值。

```html
<div v-bind:style="calcColor" class="canvas"></div>
```

`v-bind`指令冒号后的内容，则代表这个指令的参数，在Vue中，一些指令可以接受一个指令，这个指令通常就**在指令的冒号后指明**，例如例子中的`v-on:click`就是告诉Vue我们要绑定的事件名，而在这里`style`则表示我们要绑定的属性名。

#### Javascript 表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持，例如：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。

但存在的限制是，每个绑定都只能包含单个表达式，且该表达式需存在返回值。

**错误的例子：**

```html
 <!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

### 缩写

`v-`前缀的指令作为我们的常用指令，每次都要书写`v-bind:style`和`v-on:click`这样的属性未免太过繁琐，所以Vue给我们提供了这两个指令可以缩写的语法糖。

可以看到，在这个例子当中我已经使用上了`v-on`的缩写形式。

而在之后的教程中，我们也将不再使用这两个指令的完整形式，而改用缩写形式。

#### `v-bind` 缩写

```html
<!-- 完整语法 -->
<div v-bind:style="calcColor" class="canvas"></div>

<!-- 缩写 -->
<div :style="calcColor" class="canvas"></div>
```

#### `v-on` 缩写

```html
<!-- 完整语法 -->
<button v-on:click="getRGB">获取当前RGB</button>

<!-- 缩写 -->
<button @click="getRGB">获取当前RGB</button>
```

`@`和`:`在HTML中对于属性名来说都是合法的字符，并且他们不会出现在最终的渲染结果中。

---

## 计算属性

可以看出，我们在这个例子使用了一个新的属性——`computed`。

上文我们了解到，在模版中绑定Javascript表达式是很便利的，但当我们有复杂的业务逻辑的时候，单一的表达式语句并不表便于我们的开发和维护。

复杂的表达式语句会使得模版不再清晰简洁，所以我们应当采用**计算属性**来应对这些复杂的逻辑。

在上面的例子中我们就声明了一个计算属性`calcColor`，暂且不深究计算属性的实现逻辑，我们可以看出，在例子中，我们所声明的计算属性是一个函数，而这个**函数的返回值**对应到我们模版中所绑定的数据。

当`data`属性中的数据发生变动，我们的计算属性会自动重新计算出新的结果提供给模版，随后模版自动完成更新。

一切都是这么的简洁自然，这就是使用计算属性的益处！

