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
 // render() 是类组件中唯一必须实现的方法，它的作用是告诉 React 当前组件要渲染什么内容。每次 state 或 props 改变时，render() 会重新执行
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
>项目中推荐使用函数组件 + Hooks（更简洁、现代）

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

# 3. Props（属性）
 >Props 是组件之间传递数据的方式，类似于函数的参数。

## Props类型：Props 可以是任何 JavaScript 值
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

## Props的不可变性：永远不要修改 props！
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

## Props的基础用法和解构

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

# 4. 组件命名规范
## 命名规则
```js
// 正确：大写字母开头（PascalCase）
function UserProfile() { }
function BlogPost() { }
function NavBar() { }
// 错误：小写字母开头
function userProfile() { } // React 会将其视为 HTML 标签
```

## 文件命名
```js
// 推荐的文件结构
src/
├── components/
│   ├── Button.jsx          // 或 Button.js
│   ├── UserCard.jsx
│   ├── Navigation/
│   │   ├── Navigation.jsx
│   │   ├── NavItem.jsx
│   │   └── index.js        // 导出 Navigation
```

# 5. 组件的导出与导入
```js
// **导入**：默认导出可以任意命名，命名导出导入必须使用相同名字
import MyButton from './Button';        // 名字随意
import CustomButton from './Button';    // 也可以
import Button from './Button';           // 最常见

// **导出**：默认导出，命名导出，混合使用（常见于组件库）
// 1. 默认导出：每个文件只能有一个默认导出，格式用export default
// 方式1：先定义后导出
function Button({ children, onClick }) {
  return <button onClick={onClick}>{children}</button>;
}
export default Button;
// 方式2：直接导出
export default function Button({ children, onClick }) {
  return <button onClick={onClick}>{children}</button>;
}
// 方式3：匿名函数
export default ({ children, onClick }) => (
  <button onClick={onClick}>{children}</button>
);

//2. 命名导出：格式用export const/function
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export function multiply(a, b) {
  return a * b;
}
export const PI = 3.14159;

//3. 混合使用
// 默认导出主组件
export default function Button({ children }) {
  return <button>{children}</button>;
}

// 命名导出辅助组件和工具函数
export const SmallButton = ({ children }) => (
  <button style={{ fontSize: '12px' }}>{children}</button>
);
export const LargeButton = ({ children }) => (
  <button style={{ fontSize: '24px' }}>{children}</button>
);
export const buttonUtils = {
  variant: 'primary'
};

// 混合导入
import Button, { SmallButton, LargeButton, buttonUtils } from './components';

function App() {
  return (
    <div>
      <Button>默认按钮</Button>
      <SmallButton>小按钮</SmallButton>
      <LargeButton>大按钮</LargeButton>
    </div>
  );
}

//实际项目用法：一个文件一个组件
```

# 6. 组件设计原则
- 单一职责：每个组件应该只做一件事。
- 保持组件简洁：组件代码不应该超过 200-300 行。
- 合理使用 Props：不要传递过多的 props。
- 避免深层嵌套：过深的组件嵌套会导致 props drilling 问题。

```js
// 问题：props 需要层层传递
<App>
  <Layout user={user}>
    <Sidebar user={user}>
      <Menu user={user}>
        <MenuItem user={user} />
      </Menu>
    </Sidebar>
  </Layout>
</App>
// 解决：使用 Context 或状态管理
```

# 7. 组件状态
>组件可以拥有状态（state），它是组件数据的私有部分，可以用来管理动态数据。状态仅适用于类组件，或者使用 React 的 Hook 时可以在函数组件中使用。React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。
## 类组件中的状态管理
```js
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

export default Counter;
```
## 函数组件中的状态管理（使用 Hook）
```js
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

## 将生命周期方法添加到类中
>在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要。每当 Clock 组件第一次加载到 DOM 中的时候，我们都想生成定时器，这在 React 中被称为挂载。同样，每当 Clock 生成的这个 DOM 被移除的时候，我们也会想要清除定时器，这在 React 中被称为卸载。
```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
 
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
 
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
 
  tick() {
    this.setState({
      date: new Date()
    });
  }
 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Clock />
);

// 解析：componentDidMount() 与 componentWillUnmount() 方法被称作生命周期钩子。在组件输出到 DOM 后会执行 componentDidMount() 钩子，我们就可以在这个钩子上设置一个定时器。this.timerID 为定时器的 ID，我们可以在 componentWillUnmount() 钩子中卸载定时器。
// 执行顺序：
1. 当 <Clock /> 被传递给 ReactDOM.render() 时，React 调用 Clock 组件的构造函数。 由于 Clock 需要显示当前时间，所以使用包含当前时间的对象来初始化 this.state 。 我们稍后会更新此状态。

2. React 然后调用 Clock 组件的 render() 方法。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 Clock 的渲染输出。

3. 当 Clock 的输出插入到 DOM 中时，React 调用 componentDidMount() 生命周期钩子。 在其中，Clock 组件要求浏览器设置一个定时器，每秒钟调用一次 tick()。

4. 浏览器每秒钟调用 tick() 方法。 在其中，Clock 组件通过使用包含当前时间的对象调用 setState() 来调度UI更新。 通过调用 setState() ，React 知道状态已经改变，并再次调用 render() 方法来确定屏幕上应当显示什么。 这一次，render() 方法中的 this.state.date 将不同，所以渲染输出将包含更新的时间，并相应地更新 DOM。

5. 一旦 Clock 组件被从 DOM 中移除，React 会调用 componentWillUnmount() 这个钩子函数，定时器也就会被清除。
```

# 8. 数据自顶向下流动
>父组件或子组件都不能知道某个组件是有状态还是无状态，并且它们不应该关心某组件是被定义为一个函数还是一个类。这就是为什么状态通常被称为局部或封装。 除了拥有并设置它的组件外，其它组件不可访问。这通常被称为**自顶向下**或**单向数据流**。 任何状态始终由某些特定组件所有，并且从该状态导出的任何数据或 UI 只能影响树中下方的组件。

# 9. Props
>在 React 中，Props（属性）是用于将数据从父组件传递到子组件的机制，Props 是只读的，子组件不能直接修改它们，而是应该由父组件来管理和更新。state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。

## 默认Props
- 函数组件：通过设置参数默认值也可以通过defaultProps属性设置
- 类组件：defaultProps 属性为 props 设置默认值
```js
class HelloMessage extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}
 
HelloMessage.defaultProps = {
  name: 'Runoob'
};
```

## propTypes 验证
>propTypes 是 React 提供的一种工具，用于对组件的 props 进行类型检查。
在 React 中，propTypes 作为组件的一个静态属性来定义。首先，你需要安装 prop-types 包：
```bash
npm install prop-types
```
然后，在你的组件文件中引入 prop-types，并定义 propTypes 属性。
- 基础类型验证
```js
import PropTypes from 'prop-types';

function UserProfile({ name, age, isActive, email, id }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>年龄: {age}</p>
      <p>状态: {isActive ? '激活' : '未激活'}</p>
      <p>邮箱: {email}</p>
      <p>ID: {id}</p>
    </div>
  );
}

UserProfile.propTypes = {
  // 基础类型
  name: PropTypes.string,        // 字符串
  age: PropTypes.number,         // 数字
  isActive: PropTypes.bool,      // 布尔值
  email: PropTypes.string,       // 字符串
  id: PropTypes.number,          // 数字
  
  // 可选链式写法
  address: PropTypes.string,     // 可选
};

// 设置默认值（可选）
UserProfile.defaultProps = {
  name: '匿名用户',
  age: 0,
  isActive: false,
};
```

-  必填验证
```js
function RequiredExample({ username, password, email }) {
  return <div>{username} - {email}</div>;
}

RequiredExample.propTypes = {
  username: PropTypes.string.isRequired,  // 必填
  password: PropTypes.string.isRequired,  // 必填
  email: PropTypes.string,                 // 可选
};

// 使用
<RequiredExample username="john" password="123456" />  // ✅ 正确
<RequiredExample password="123456" />                   // ❌ 警告（缺少 username）
```

- 数组和对象验证
```js
function DataDisplay({ items, user, settings }) {
  return (
    <div>
      {items.map(item => <li key={item}>{item}</li>)}
      <p>用户: {user.name}</p>
    </div>
  );
}

DataDisplay.propTypes = {
  // 数组
  items: PropTypes.array,                    // 任意数组
  tags: PropTypes.arrayOf(PropTypes.string), // 字符串数组
  numbers: PropTypes.arrayOf(PropTypes.number), // 数字数组
  
  // 对象
  user: PropTypes.object,                    // 任意对象
  user: PropTypes.shape({                    // 特定形状的对象
    name: PropTypes.string.isRequired,
    age: PropTypes.number,
    email: PropTypes.string
  }),
  
  // 对象实例
  date: PropTypes.instanceOf(Date),          // Date 实例
  
  // 对象值类型验证
  settings: PropTypes.objectOf(PropTypes.number), // 对象的值都是数字
};

// 使用示例
<DataDisplay 
  items={['a', 'b', 'c']}
  tags={['react', 'javascript']}
  numbers={[1, 2, 3]}
  user={{ name: '张三', age: 25 }}
  date={new Date()}
  settings={{ width: 100, height: 200 }}
/>
```

-  函数和元素验证
```js
function InteractiveComponent({ onClick, renderHeader, children, icon }) {
  return (
    <div>
      {renderHeader()}
      <button onClick={onClick}>{children}</button>
      {icon}
    </div>
  );
}

InteractiveComponent.propTypes = {
  onClick: PropTypes.func,                    // 函数
  renderHeader: PropTypes.func,               // 函数
  children: PropTypes.node,                   // 可渲染的节点（任何可渲染内容）
  icon: PropTypes.element,                    // React 元素（必须是一个元素）
  
  // 更多类型
  elementType: PropTypes.elementType,         // React 组件类型
  message: PropTypes.node,                    // 字符串、数字、元素等
};

// 使用
<InteractiveComponent 
  onClick={() => console.log('clicked')}
  renderHeader={() => <h1>Header</h1>}
  icon={<span>🔔</span>}
>
  Click me
</InteractiveComponent>
```

>注意： 现在的 React 项目更推荐使用 TypeScript 替代 PropTypes，因为 TypeScript 能在编译时发现问题，提供更好的开发体验。



