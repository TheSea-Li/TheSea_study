# 1. 什么是JSX？
JSX = JavaScript + XML，是 JavaScript 的语法扩展，专门用来在 React 中写 UI 结构。

# 2. JSX语法规则
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

# 3. JSX本质
JSX 只是 React.createElement 的语法糖，浏览器最终执行的是普通 JS
```jsx
// 你写的 JSX
<div className="box">Hello</div>

// 编译后（React 自动处理）
React.createElement("div", { className: "box" }, "Hello");
```



