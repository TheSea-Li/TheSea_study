# 1. 什么是 React Hooks？
> React 16.8 推出的钩子函数，让函数组件可以拥有：状态管理、生命周期、DOM 操作、跨组件通信 等功能。
>> 以前的函数组件 = 只能渲染 UI，没状态、没生命周期（哑巴组件）
>> Hooks = 给函数组件装功能插件
>> 所有 Hooks 都以 use 开头
>> 现在写 React 100% 用 Hooks + 函数组件，彻底抛弃类组件

# 2. Hooks 3 条铁律
1. 只能在函数组件 / 自定义 Hooks 里使用
2. 只能写在组件顶层（不能放在 if /for/ 嵌套函数里）
3. 命名必须以 use 开头

# 3. 三大核心Hooks
- useState → 响应式状态管理
> 作用：给组件添加响应式数据，数据变 → 页面自动刷新
```jsx
import { useState } from 'react';

function Demo() {
  // 语法：[状态变量, 修改状态的方法] = useState(初始值)
  const [num, setNum] = useState(0);

  return (
    <div>
      <p>{num}</p>
      {/* 修改状态：必须用 set 方法，触发页面更新 */}
      <button onClick={() => setNum(num + 1)}>+1</button>
    </div>
  );
}
```

- useEffect → 副作用 / 生命周期
```jsx
import { useEffect } from 'react';

useEffect(() => {
  // 1. 执行逻辑（挂载/更新时执行）
  console.log("组件挂载/更新");

  // 2. 清理函数（卸载/更新前执行）
  return () => console.log("清理/卸载");
}, [依赖项]); // 依赖项控制执行时机
```
> [] → 仅挂载执行 1 次
> [num] → num 变化时更新执行
> 无依赖 → 每次渲染都执行

- useRef → 操作 DOM / 存不渲染变量
> 作用1：获取真实 DOM 元素。
> 作用 2：存储变量，修改不触发页面重渲染
```jsx
import { useRef } from 'react';

function Demo() {
  const inputRef = useRef(null);

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>聚焦</button>
    </div>
  );
}
```

# 3. 进阶Hooks
1. useMemo → 缓存计算结果
> 作用：避免复杂计算重复执行，优化性能。

2. useCallback → 缓存函数
> 作用：防止函数重复创建，避免子组件重渲染。

3. useContext → 跨组件传值
> 作用：解决 props 层层传递（祖孙传值）。

4. useNavigate/useParams → 路由 Hooks
> 作用：React 路由专用，跳转 / 拿参数。

# 4. 自定义Hooks
> 把重复逻辑封装成自己的 Hooks，实现逻辑复用。

示例：封装一个请求数据的自定义 Hook
```jsx
// 自定义 Hooks：useRequest
import { useState, useEffect } from 'react';

function useRequest(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url).then(res => res.json()).then(data => {
      setData(data);
      setLoading(false);
    });
  }, [url]);

  return { data, loading };
}

// 组件中使用
function User() {
  const { data, loading } = useRequest("https://api.xxx.com/users");
  return loading ? <p>加载中</p> : <div>{data.name}</div>;
}
```