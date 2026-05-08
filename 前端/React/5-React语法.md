# 1. 事件处理
> React 元素的事件处理和 DOM 元素类似。但是有一点语法差异和特性。
## 基本语法对比
**主要区别**
- 事件名使用驼峰命名（onClick 而非 onclick）
- 传入函数引用（{handleClick} 而非字符串）
```jsx
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

## 传递参数
```jsx
function UserList() {
  const users = ['Alice', 'Bob', 'Charlie'];
  
  // 方式1：箭头函数包装（推荐）
  const handleDelete = (userName) => {
    console.log(`删除用户：${userName}`);
  };
  
  return (
    <ul>
      {users.map(user => (
        <li key={user}>
          {user}
          <button onClick={() => handleDelete(user)}>删除</button>
        </li>
      ))}
    </ul>
  );
}
```

## 事件对象
> React 使用合成事件（SyntheticEvent），是对原生事件的跨浏览器封装。
```jsx
function FormHandler() {
  const handleClick = (e) => {
    e.preventDefault();        // 阻止默认行为
    e.stopPropagation();       // 阻止冒泡
    console.log('事件类型:', e.type);
    console.log('目标元素:', e.target);
    console.log('当前元素:', e.currentTarget);
  };
  
  const handleInput = (e) => {
    console.log('输入的值:', e.target.value);
    console.log('输入框名称:', e.target.name);
  };
  
  return (
    <div onClick={handleClick}>
      <button onClick={handleClick}>点击</button>
      <input 
        name="username"
        onChange={handleInput} 
        placeholder="请输入用户名"
      />
    </div>
  );
}
```

##类组件中的事件处理（this 绑定）
```jsx
class Counter extends React.Component {
  state = { count: 0 };
  
  // 方式1：箭头函数（推荐）
  handleClick = () => {
    this.setState({ count: this.state.count + 1 });
  };
  
  // 方式2：构造函数中绑定
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState({ count: this.state.count + 1 });
  }
  
  // 方式3：render 中绑定（不推荐，性能差）
  render() {
    return (
      <button onClick={this.handleClick.bind(this)}>
        计数: {this.state.count}
      </button>
    );
  }
  
  // 方式4：箭头函数包装（不推荐，性能差）
  render() {
    return (
      <button onClick={() => this.handleClick()}>
        计数: {this.state.count}
      </button>
    );
  }
}
```

# 2. 条件渲染
> 条件渲染是 React 中根据不同状态显示不同 UI 的技术，类似于 JavaScript 中的 if 或三元运算符。
**if语句**
```jsx
function UserGreeting({ isLoggedIn }) {
  // ✅ 方式1：if 语句（适合复杂逻辑）
  if (isLoggedIn) {
    return <h1>欢迎回来！</h1>;
  } else {
    return <h1>请先登录</h1>;
  }
}

// 或者更简洁
function UserGreeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>欢迎回来！</h1>;
  }
  return <h1>请先登录</h1>;
}
```

**三元运算符**
```jsx
function UserStatus({ isLoggedIn }) {
  return (
    <div>
      {/* ✅ 基础用法 */}
      {isLoggedIn ? <span>已登录</span> : <span>未登录</span>}
      
      {/* ✅ 渲染不同组件 */}
      {isLoggedIn ? <Dashboard /> : <LoginPage />}
      
      {/* ✅ 复杂样式 */}
      <div className={isLoggedIn ? 'user-active' : 'user-inactive'}>
        {isLoggedIn ? 'VIP会员' : '普通用户'}
      </div>
    </div>
  );
}
```

**逻辑与（&&）运算符**
```jsx
function Notification({ hasMessage, message }) {
  return (
    <div>
      {/* ✅ 有消息时才显示 */}
      {hasMessage && <div className="alert">{message}</div>}
      
      {/* ✅ 条件显示徽章 */}
      {hasMessage && <Badge count={message.length} />}
      
      {/* ⚠️ 注意：0 会被渲染！ */}
      {hasMessage && <span>新消息</span>}  {/* 正确 */}
      {0 && <span>不会显示</span>}  {/* 错误：会显示 0 */}
    </div>
  );
}

// 陷阱示例
function Counter({ count }) {
  return (
    <div>
      {/* ❌ 危险：count 为 0 时会显示 0 */}
      {count && <span>计数: {count}</span>}
      
      {/* ✅ 安全：使用三元运算符 */}
      {count > 0 && <span>计数: {count}</span>}
      
      {/* ✅ 或者转为布尔值 */}
      {!!count && <span>计数: {count}</span>}
    </div>
  );
}
```

**立即执行函数**
```jsx
function ComplexComponent({ status, user }) {
  return (
    <div>
      {/* ✅ 复杂逻辑可以使用 IIFE */}
      {(() => {
        if (status === 'loading') {
          return <LoadingSpinner />;
        } else if (status === 'error') {
          return <ErrorMessage />;
        } else if (user.isVIP) {
          return <VIPDashboard user={user} />;
        } else {
          return <NormalDashboard user={user} />;
        }
      })()}
    </div>
  );
}
```

**变量存储**
```jsx
function Dashboard({ user, status, permissions }) {
  // ✅ 将不同情况的内容存储到变量
  let content;
  
  if (status === 'loading') {
    content = <LoadingSpinner />;
  } else if (status === 'error') {
    content = <ErrorMessage />;
  } else if (!user) {
    content = <LoginPrompt />;
  } else if (!permissions.canViewDashboard) {
    content = <AccessDenied />;
  } else {
    content = (
      <div>
        <WelcomeBanner user={user} />
        <StatsWidget permissions={permissions} />
        <RecentActivity user={user} />
      </div>
    );
  }
  
  return (
    <div className="dashboard">
      <Header />
      {content}
      <Footer />
    </div>
  );
}
```

**Switch 语句模式**
```jsx
function StatusRenderer({ status, data }) {
  // ✅ 使用 switch 处理多种状态
  const renderContent = () => {
    switch(status) {
      case 'idle':
        return <StartScreen />;
      case 'loading':
        return <LoadingSpinner />;
      case 'success':
        return <DataDisplay data={data} />;
      case 'error':
        return <ErrorAlert />;
      default:
        return null;
    }
  };
  
  return (
    <div className="status-container">
      {renderContent()}
    </div>
  );
}

// 对象映射方式（更优雅）
const STATUS_COMPONENTS = {
  idle: StartScreen,
  loading: LoadingSpinner,
  success: DataDisplay,
  error: ErrorAlert
};

function StatusRenderer({ status, data }) {
  const Component = STATUS_COMPONENTS[status];
  
  if (!Component) return null;
  
  return (
    <div className="status-container">
      <Component data={data} />
    </div>
  );
}
```

**实际项目示例（多个条件）**
```jsx
function ShoppingCart({ cart, user, loading, error }) {
  // 1. 加载状态
  if (loading) {
    return <LoadingSpinner message="加载购物车中..." />;
  }
  
  // 2. 错误状态
  if (error) {
    return (
      <ErrorMessage 
        message={error.message}
        onRetry={() => window.location.reload()}
      />
    );
  }
  
  // 3. 空状态
  if (!cart || cart.items.length === 0) {
    return (
      <EmptyCart 
        message="购物车是空的"
        onContinueShopping={() => navigate('/products')}
      />
    );
  }
  
  // 4. 正常显示
  return (
    <div className="cart">
      <h2>购物车 ({cart.items.length} 件商品)</h2>
      
      {cart.items.map(item => (
        <CartItem key={item.id} item={item} />
      ))}
      
      {/* 条件显示优惠信息 */}
      {user.isVIP && cart.total > 100 && (
        <VIPDiscount amount={cart.total * 0.1} />
      )}
      
      {/* 条件显示运费 */}
      {cart.total < 50 ? (
        <ShippingFee amount={10} />
      ) : (
        <FreeShipping />
      )}
      
      {/* 条件显示支付方式 */}
      <PaymentMethods>
        {cart.total > 1000 && <InstallmentPayment />}
        {user.hasCreditCard && <CreditCardPayment />}
      </PaymentMethods>
    </div>
  );
}
```

# 3. 列表
> React 没有专门的列表指令（如 Vue 的 v-for），而是直接使用 JavaScript 原生的 map() 方法遍历数组，将数据渲染成一组 UI 元素。
>>核心：必须掌握的 key 属性：
>>1. 什么是key? -> key 是 React 用于识别列表中每个元素的唯一标识，是列表渲染的必填属性。
>>2. 为什么必须用key? -> React 的 Diff 算法 会通过 key 判断：
>> 哪个元素被修改 / 删除 / 新增；
>> 避免重复渲染，提升列表性能；
>> 防止列表状态错乱（比如复选框、输入框的状态错位）。
>>3. key的使用规则：
>> 1. 唯一：在当前列表中，key 值不能重复；
>> 2. 稳定：key 值不会随数据排序、增删而改变；
>> 3. 优先使用后端返回的唯一 ID（如数据的 id 字段）。

**代码示例**
```jsx
import React from 'react';

function ProductList() {
  // 真实接口返回的对象数组
  const products = [
    { id: 101, name: 'React 实战教程', price: 99, stock: 100 },
    { id: 102, name: 'JavaScript 高级程序设计', price: 129, stock: 50 },
    { id: 103, name: 'Vue 入门到精通', price: 89, stock: 80 },
  ];

  return (
    <div className="product-list">
      <h2>商品列表</h2>
      <table border="1" cellPadding="8">
        <thead>
          <tr>
            <th>商品ID</th>
            <th>商品名称</th>
            <th>价格</th>
            <th>库存</th>
          </tr>
        </thead>
        <tbody>
          {/* 遍历对象数组，渲染表格行 */}
          {products.map(item => (
            <tr key={item.id}>
              <td>{item.id}</td>
              <td>{item.name}</td>
              <td>¥{item.price}</td>
              <td>{item.stock}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default ProductList;
```

**组件化（封装列表项）**
>大型项目中，不会把所有逻辑写在一个组件里，会拆分列表项为独立组件，代码更易维护。
```jsx
// 子组件：列表项
function TodoItem({ todo }) {
  return (
    <div style={{ margin: '8px 0' }}>
      <span>{todo.title}</span>
      <span style={{ color: todo.completed ? 'green' : 'red', marginLeft: '10px' }}>
        {todo.completed ? '已完成' : '未完成'}
      </span>
    </div>
  );
}

// 父组件：列表容器
function TodoList() {
  const todos = [
    { id: 1, title: '学习 React 列表', completed: true },
    { id: 2, title: '写一个待办项目', completed: false },
  ];

  return (
    <div>
      <h2>待办列表</h2>
      {todos.map(todo => (
        // key 写在遍历的最外层元素/组件上
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </div>
  );
}

export default TodoList;
```

**动态列表（增、删、改）**
> 列表最常用的功能是动态操作数据，配合 React 的 useState 实现。
```jsx
import React, { useState } from 'react';

function DynamicTodoList() {
  // 1. 定义列表状态
  const [todos, setTodos] = useState([
    { id: 1, title: '学习React列表', completed: false },
    { id: 2, title: '掌握key属性', completed: true },
  ]);
  // 输入框状态
  const [inputValue, setInputValue] = useState('');

  // 2. 新增待办
  const addTodo = () => {
    if (!inputValue.trim()) return;
    const newTodo = {
      id: Date.now(), // 用时间戳生成唯一id
      title: inputValue,
      completed: false,
    };
    // 核心：不能直接修改原数组，必须返回新数组
    setTodos([...todos, newTodo]);
    setInputValue('');
  };

  // 3. 删除待办
  const deleteTodo = (id) => {
    // filter 过滤掉要删除的项，返回新数组
    setTodos(todos.filter(item => item.id !== id));
  };

  // 4. 切换完成状态
  const toggleTodo = (id) => {
    setTodos(
      todos.map(item => 
        item.id === id ? { ...item, completed: !item.completed } : item
      )
    );
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>动态待办列表</h2>
      {/* 新增输入框 */}
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        placeholder="输入待办事项"
      />
      <button onClick={addTodo} style={{ marginLeft: '8px' }}>添加</button>

      {/* 渲染列表 */}
      <div style={{ marginTop: '20px' }}>
        {todos.map(item => (
          <div key={item.id} style={{ margin: '8px 0' }}>
            <input
              type="checkbox"
              checked={item.completed}
              onChange={() => toggleTodo(item.id)}
            />
            <span style={{ 
              textDecoration: item.completed ? 'line-through' : 'none',
              margin: '0 8px'
            }}>
              {item.title}
            </span>
            <button onClick={() => deleteTodo(item.id)}>删除</button>
          </div>
        ))}
      </div>
    </div>
  );
}

export default DynamicTodoList;
```

**总结**
> 1. React 列表渲染 = 数组.map () + 唯一 key；
> 2. key 是核心：必须唯一、稳定，优先用数据 id；
> 3. 状态不可变：操作列表（增删改）必须返回新数组，不修改原数据；
> 4. 工程化：拆分列表项为独立组件，提升代码可维护性。











