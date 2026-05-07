# 创建项目
使用前端构建工具Vite，官方推荐首选 
```bash
#创建指令
npm create vite@latest first-react-app -- --template react
```
- create vite@latest：使用最新版Vite创建工具
- first-react-app：项目名称（推荐写法：小写字母+连字符）
- -- --template react：指定React模板
# 启动项目
```bash
#安装依赖
npm i

# 运行
npm run dev
```
# 项目结构
![目录结构](../Images/React_image6.png)
**核心文件解析**
- index.html：React应用的==HTML模板文件==。React会将组件渲染到<div id="root"></div>中。
- index.js：React应用的==入口文件==，负责将根组件渲染到index.html中的div#root中。
- App.js：React应用的==根组件==，包含应用的主要逻辑和结构。
- App.css： App组件的==样式文件==。
- index.css：==全局样式文件==，定义全局样式或充值浏览器默认样式。
- package.json：项目==配置文件==，包含项目的元数据，依赖和脚本。

**React组件的基本结构**
1.函数组件
![组件基本结构](../Image/React_image7.png)
说明：
- import React from 'react';: 导入了 React 库。
- function MyComponent() { ... }: 这是一个函数组件。函数组件是一个返回 JSX 的 JavaScript 函数。
- return (...): 组件的 return 语句返回一个 JSX 元素。JSX 是 JavaScript 的语法扩展，允许你在 JavaScript 中编写类似 HTML 的代码。
- export default MyComponent;: 将 MyComponent 组件导出为默认导出。这样，其他文件可以通过 import MyComponent from './MyComponent'; 来导入并使用这个组件。

2. JSX
```bash
# JSX 是 JavaScript 的语法扩展，允许在 JavaScript 中编写类似 HTML 的代码。
const element = <h1>Hello, JSX!</h1>;
```

3.Props
```bash
# Props 是组件的输入参数，用于从父组件向子组件传递数据。
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return <Welcome name="React" />;
}
```

4.State
```bash
# State 是组件的内部状态，用于管理组件的数据。
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

# 入口文件
```base
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

# ReactDOM.createRoot：React 18 新 API，启用并发特性。
# <App />：渲染你的主组件。
# <React.StrictMode>：开发模式下严格检查，帮助发现问题。

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```
>JSX 是 JavaScript 的语法扩展，允许你在 JavaScript 中编写类似 HTML 的代码。

# 开发React
**1. 工具选择：Visual Studio Code**
- 智能补全
- 插件丰富
- 集成终端
- 调试工具
- 跨平台：Windows，macOS，Linux都完美支持
- 免费开源

**2. 必备插件**
- ESLint：实时检查代码规范
- Prettier - Code formatter：自动格式化代码，保持风格统一
- ES7 + React/Redux/React-Native snippets：输入快捷键快速生成组件
- Path Intellisense：导入路径自动补全（import时）
- GitLens：GIt增强，查看代码提交历史
>安装完成后，重启VS Code

**3. 配置VS Code**
- 想要在保存时自动格式化只装插件不够，项目里必须安装Prettier

```base
# 安装指令
npm install --save-dev prettier eslint-config-prettier
```
- 在项目根目录创建.vscode文件夹，在该文件夹下创建settings.json（推荐项目级配置）文件，并添加以下内容
```base
{
  # 保存时自动格式化
  "editor.formatOnSave": true,
  # 默认使用 Prettier 格式化
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  # ESLint 自动修复
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },

  # JSX/TSX 文件也应用 Prettier
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },

  # 显示行号、缩进指南
  "editor.rulers": [80],
  "editor.guides.indentation": true
}
```
>保存后生效。现在每次保存代码，Prettier 会自动格式化，ESLint 会自动修复常见问题。

**4.调试React代码**
1. 安装浏览器插件 React Developer Tools。
2. 在 VS Code，按 F5 启动调试（需先配置 launch.json，但入门阶段先用浏览器 DevTools）。
3. 在浏览器 F12 → Components 标签查看你的 Greeting 组件的 Props 和 state。

**5. 常用快捷键**
- Ctrl + S：保存
- Ctrl + /：注释行
- Alt + ↑/↓：移动行
- Ctrl + D：多光标选中相同词
- Ctrl + Shift + P：命令面板（万能）