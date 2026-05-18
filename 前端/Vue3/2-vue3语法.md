# 一、Vue 单文件组件
Vue 单文件组件是 Vue.js 框架的文件格式，它允许开发者将 HTML、JavaScript 和 CSS 代码放在一个文件中，通常以 .vue 为文件后缀。

# 二、模板语法
### 插值
Vue.js 使用双大括号 {{ }} 来表示文本插值：

### 指令
指令是带有前缀 v- 的特殊属性，用于在模板中表达逻辑。
- **v-bind**: 动态绑定一个或多个特性，或一个组件 prop。
```js
<a v-bind:href="url">Link</a>
```
简写：
```js
<a :href="url">Link</a>
```

- **v-if / v-else-if / v-else**: 条件渲染。
```js
<p v-if="visible">内容可见</p>
<p v-else>内容不可见</p>
```

- **v-for**: 列表渲染。
```js
<ul>
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
</ul>
```

- **v-model**: 双向数据绑定。
```js
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

- **v-on**: 事件监听器。
```js
<button v-on:click="doSomething">Click me</button>
```
简写
```js
<button @click="doSomething">Click me</button>
```

# 三、事件处理
在 Vue.js 中，可以使用 v-on 指令来监听 DOM 事件，并在触发时执行一些 JavaScript 代码。
```js
<div id="app">
  <button @click="greet">Greet</button>
</div>

<script>
    methods: {
      greet() {
        alert('Hello!');
      }
    }
</script>
```

# 四、 计算属性
计算属性是基于其依赖进行缓存的属性。计算属性只有在其相关依赖发生变化时才会重新计算。

# 五、组件
组件是 Vue.js 最强大的功能之一。组件允许你使用小型、独立和通常可复用的组件构建大型应用。

# 六、Props 和事件
- Props 用于在组件之间传递数据。
- 子组件通过 $emit 触发事件，父组件可以监听这些事件。















