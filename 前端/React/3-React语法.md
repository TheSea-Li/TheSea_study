# 1.JSX语法规则
- 必须闭合标签
- class->className：因为class是js关键字，故用className替代
- for->htmlFor
- 大写开头组件
```js
# 正确写法
<Card /> 

# 错误写法
<card />
```

- 注释写法：{/* 注释内容 */}
>注意：根元素内部才使用这种注释写法，外部采用/* 注释内容 */。

- 根元素：return只能有一个根节点
```js
return (
  <>  {/* 碎片 Fragment，空标签，不渲染 DOM */}
    <h1>标题</h1>
    <p>段落</p>
  </>
);
```

# 2.表达式嵌入，条件渲染，列表渲染
>JSX 最强大的地方是可以用 {} 嵌入 JavaScript 表达式（只能是表达式，不能是语句如 if/for）。

- **表达式嵌入**：任何有效的 JS 表达式都可以放进 {}
```js
const name = 'runoob';
const now = new Date().toLocaleString();

return (
  <div>
    <h1>你好，{name}！</h1>
    <p>当前时间：{now}</p>
    <p>计算结果：{2 + 3 * 5}</p>  {/* 17 */}
  </div>
);
```

- **条件渲染**：用三元运算符或 &&，不要用 if 语句直接在 JSX 中（可以用在函数体里）
```js
const isLoggedIn = true;
const count = 0;

return (
  <div>
    {isLoggedIn ? <h1>欢迎回来，{name}！</h1> : <h1>请登录</h1>}
    
    {count > 0 && <p>你有 {count} 条未读消息</p>}  {/* count 为 0 时不渲染 */}
  </div>
);
```

- **列表渲染**：用 map() 渲染数组，必须给每个元素加 key 属性（唯一标识，帮助 React 高效更新 DOM）。
```js
const items = [
  { id: 1, name: '苹果' },
  { id: 2, name: '香蕉' },
  { id: 3, name: '橙子' },
];

return (
  <ul>
    {items.map(item => (
      <li key={item.id}>  {/* key 必须唯一且稳定！ */}
        {item.name}
      </li>
    ))}
  </ul>
);
```
>key的重要性
>>- 没有 key：React 会警告，可能导致渲染错误（尤其是列表动态变化时）。
>>- 坏习惯：用 index 作为 key（key={index}）——当列表顺序变化时，会导致不必要的重渲染或状态丢失。
>>- 最佳实践：用数据中的唯一 ID（如数据库 id）。

#3. 样式处理
- 内联样式
```js
return (
  <div
    style={{
      backgroundColor: '#f0f0f0',
      padding: '20px',
      borderRadius: '8px',
      textAlign: 'center',
      fontFamily: 'Arial, sans-serif',
    }}
  >
    <h2 style={{ color: 'blue', marginBottom: '10px' }}>
      内联样式示例
    </h2>
  </div>
);
```
>注意：内联样式是用{{}}包裹起来

- CSS Modules：Vite 默认支持 CSS Modules，避免全局污染。
```js
/* Card.module.css */
.card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  padding: 20px;
  margin: 20px;
}

.title {
  color: #333;
  margin: 0 0 10px 0;
}

.content {
  color: #666;
}
```
在组件中导入并使用
```js
import styles from './styles/Card.module.css';

return (
  <div className={styles.card}>
    <h3 className={styles.title}>标题</h3>
    <p className={styles.content}>内容</p>
  </div>
);
```