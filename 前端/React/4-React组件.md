# 1. 组件定义
React 组件是构建 React 应用的基本单元，可以把组件理解为可复用的、独立的 UI 单元，每个组件封装了自己的结构、样式、逻辑和状态。组件可以小到一个按钮，也可以大到整个页面。

# 2. 组件分类
## 函数组件
>函数组件是一个接受 props 并返回 React 元素（通常是JSX）的 JavaScript 函数。
```js
// 示例 （三种函数声明方式任选一种）
import React from 'react';

// 方式 1：函数声明
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 方式 2：箭头函数
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
};

// 方式 3：简化写法（单行返回）
const Welcome = (props) => <h1>Hello, {props.name}</h1>;

export default Welcome;
```

## 类组件
>类组件使用 ES6 类语法定义，通常用于需要管理状态或使用生命周期方法的情况。
```js
// 示例
import React, { Component } from 'react';

class Welcome extends Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

export default Welcome;
```

## 复合组件
>通过创建多个组件来合成一个组件，即把组件的不同功能点进行分离。
```js
function Name(props) {
    return <h1>网站名称：{props.name}</h1>;
}
function Url(props) {
    return <h1>网站地址：{props.url}</h1>;
}
function Nickname(props) {
    return <h1>网站小名：{props.nickname}</h1>;
}
function App() {
    return (
    <div>
        <Name name="菜鸟教程" />
        <Url url="http://www.runoob.com" />
        <Nickname nickname="Runoob" />
    </div>
    );
}
 
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
     <App />
);
```

## 测试实例
>接下来封装一个输出 "Hello World！" 的组件，组件名为 HelloMessage
```js
function HelloMessage(props) {
    return <h1>Hello World!</h1>;
}
 
const element = <HelloMessage />;
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
    element
);
```

> ***注意，原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。***

# 3.Props（属性）
 >Props 是组件之间传递数据的方式，类似于函数的参数。

- Props类型：Props 可以是任何 JavaScript 值
```js
function Demo() {
  const user = { name: "Alice", age: 25 };
  const numbers = [1, 2, 3, 4, 5];
  const handleClick = () => alert("Clicked!");
  return (
    <MyComponent
      // 字符串
      title="Hello"
      // 数字
      count={42}
      // 布尔值
      isActive={true}
      // 数组
      items={numbers}
      // 对象
      user={user}
      // 函数
      onClick={handleClick}
      // JSX
      children={<p>This is content</p>}
    />
  );
}
```

- Props的不可变性：永远不要修改 props！
```js
// 错误：修改 props
function BadComponent(props) {
  props.name = "Changed"; // 绝对不要这样做！
  return <h1>{props.name}</h1>;
}
// 正确：将 props 视为只读
function GoodComponent({ name }) {
  const displayName = name.toUpperCase(); // 创建新值
  return <h1>{displayName}</h1>;
}
```

- Props的基础用法和解构

1. 基础用法
```js
// 父组件传递 props
function App() {
  return (
    <div>
      <Greeting name="Alice" age={25} />
      <Greeting name="Bob" age={30} />
    </div>
  );
}
// 子组件接收 props
function Greeting(props) {
  return (
    <div>
      <h1>Hello, {props.name}!</h1>
      <p>Age: {props.age}</p>
    </div>
  );
}
```

2. 解构用法
```js
// 推荐：直接解构
function Greeting({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Age: {age}</p>
    </div>
  );
}
// 带默认值的解构
function Button({ text = "Submit", variant = "primary", disabled = false }) {
  return (
    <button className={variant} disabled={disabled}>
      {text}
    </button>
  );
}
```








