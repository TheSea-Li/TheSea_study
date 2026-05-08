# 1. 事件处理
> React 元素的事件处理和 DOM 元素类似。但是有一点语法差异和特性。
## 基本语法对比
** 主要区别 **
- 事件名使用驼峰命名（onClick 而非 onclick）
- 传入函数引用（{handleClick} 而非字符串）
```js
// 原生 HTML
<button onclick="handleClick()">点击</button>

// React JSX（注意：驼峰命名）
<button onClick={handleClick}>点击</button>
```

## 事件处理器定义方式
```jsx
function Button() {
  // 方式1：单独定义函数（推荐）
  function handleClick() {
    console.log('按钮被点击了');
  }
  
  // 方式2：箭头函数
  const handleClick = () => {
    console.log('按钮被点击了');
  };
  
  return <button onClick={handleClick}>点击</button>;
}
```























