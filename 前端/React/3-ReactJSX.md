# 1. React 元素渲染
### 1. 分清3个概念
- 真实 DOM 元素：浏览器里的 HTML 标签（div/span/h1），看得见摸得着
- React 元素：用 JSX 写的对象（<h1>你好</h1>），是虚拟 DOM（只是一个 JS 对象，不是真 DOM）
- React 组件：你写的 function Greeting()，返回一堆 React 元素

### 2. 什么是React 元素渲染
> 把「JSX 写的 React 元素」，变成「浏览器能显示的真实 DOM」，放到页面上的过程。

### 3. 渲染的入口文件
>所有 React 项目，渲染都从 src/index.js 开始（React 18 最新语法）
```jsx
// 1. 引入核心库
import React from 'react';
// 2. 引入渲染方法
import { createRoot } from 'react-dom/client';
// 3. 引入你的根组件
import App from './App';

// 4. 找到页面上的【真实DOM根容器】（public/index.html 里的 <div id="root"></div>）
const root = createRoot(document.getElementById('root'));

// 5. 核心：渲染！把根组件渲染到根容器里（整个 React 项目的渲染起点！）
root.render(<App />); 
```
### 4. 渲染的完整流程
```jsx
export default function Greeting() {
  return (
    <div>
      <h2>排行榜</h2>
      <li>张三</li>
    </div>
  );
}
```
渲染步骤：
1. 编译 JSX：<div><h2>排行榜</h2>...</div> 变成 React 元素（虚拟 DOM 对象）
2. React 解析：读取这个虚拟对象的结构
3. 生成真实 DOM：把对象转成浏览器认识的 HTML 标签
4. 挂载到页面：放到 id="root" 的容器里，显示出来

### 5. 两种渲染方式
1. 首次渲染（挂载）
页面刚打开，组件第一次显示 → 从头渲染所有 DOM
2. 更新渲染（重渲染）
state/ props 发生变化 → React 不会重新渲染全部 DOM，只会局部更新变化的部分！

### 6. React渲染的3个特性
- **声明式渲染**：只管写 JSX（图纸），React 自动帮你生成 DOM，不用手动操作 document.createElement
- **虚拟 DOM 优化**：不直接操作真实 DOM，用虚拟 DOM 对比，最小化更新，性能远超原生 JS
- **数据驱动渲染**：页面变 → 不是改 DOM → 而是改 state → React 自动重新渲染

### 7. React渲染总结
JSX 生成 React 元素 → 组件返回元素 → root.render 渲染元素 → state 变化触发更新渲染 → 页面自动刷新！

# 2. 什么是JSX？
JSX = JavaScript + XML，是 JavaScript 的语法扩展，专门用来在 React 中写 UI 结构。

# 3. JSX语法规则
### 1. 必须有唯一根节点
一个组件的 JSX 只能有一个顶层标签，不能并列多个根元素。
```jsx
// 正确写法
export default function Demo() {
  return (
    <> {/* 根节点：空Fragment */}
      <h1>标题</h1>
      <p>内容</p>
    </>
  );
}
```
```jsx
// 错误写法
return (
  <h1>标题</h1>
  <p>内容</p>
);
```

### 2. 用 className 代替 class
因为 class 是 JS 关键字，所以 JSX 必须用 className 定义样式类
```jsx
<div className="box">样式</div>
```

### 3. 用 htmlFor 代替 for
标签的 for 属性改成 htmlFor
```jsx
<label htmlFor="username">用户名</label>
<input id="username" />
```

### 4. 所有标签必须闭合
```jsx
<img src="logo.png" />
<input type="text" />
<br />
```

### 5. JS 中嵌入表达式用 { }
在 JSX 中写变量、运算、函数、字符串、JS 逻辑，必须包在大括号里：
```jsx
const name = "张三";
const num = 10;

return (
  <div>
    {/* 变量 */}
    <p>{name}</p>
    {/* 运算 */}
    <p>{num + 5}</p>
    {/* 字符串 */}
    <p>{"你好世界"}</p>
  </div>
);
```


### 6. 行内样式用 双重大括号 {{ }}
第一个 {} 表示写 JS，第二个 {} 表示样式对象：
```jsx
<div style={{ color: "red", fontSize: "20px" }}>行内样式</div>
```

### 7.  事件名驼峰命名
onClick、onChange、onSubmit，不能小写
```jsx
<button onClick={() => alert("点击")}>按钮</button>
```


### 8.  注释写法：{/* 注释内容 */}
JSX 里不能用 //，必须用大括号包起来
```jsx
<div>
  {/* 这是 JSX 注释 */}
  <p>内容</p>
</div>
```

### 9.  条件渲染（不能用 if/else）
JSX 不支持直接写 if/else，用 三元运算符 或 逻辑与 &&
```jsx
const isLogin = true;

return (
  <div>
    {/* 三元运算 */}
    {isLogin ? <p>欢迎回来</p> : <p>请登录</p>}
    
    {/* 逻辑与 && */}
    {isLogin && <button>退出登录</button>}
  </div>
);
```

### 10.  列表渲染：map + key
```jsx
const data = [{id:1,name:"张三"},{id:2,name:"李四"}]

return (
  <ul>
    {data.map(item => (
      <li key={item.id}>{item.name}</li>
    ))}
  </ul>
);
```

# 4. JSX本质
JSX 只是 React.createElement 的语法糖，浏览器最终执行的是普通 JS
```jsx
// 你写的 JSX
<div className="box">Hello</div>

// 编译后（React 自动处理）
React.createElement("div", { className: "box" }, "Hello");
```



