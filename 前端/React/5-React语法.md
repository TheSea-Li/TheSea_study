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














