# 一. 核心最重要：减少不必要组件重渲染
### 1. React.memo 缓存函数组件
- 作用：props 没变就不重渲染，做浅层对比
- 用法：直接包裹子组件
```jsx
export default React.memo(Child);
```

### 2. useCallback 缓存函数
- 问题：函数组件每次渲染都会生成全新函数，导致 memo 失效
- 作用：缓存函数引用，依赖不变就复用旧函数
```jsx
const handleClick = useCallback((id) => {
  console.log(id);
}, []);
```

### 3. useMemo 缓存计算结果 / 缓存组件
- 作用：缓存复杂计算，避免每次渲染重复计算
```jsx
const total = useMemo(() => {
  return list.reduce((sum, item) => sum + item.price, 0);
}, [list]);
```

### 4. 禁止直接传字面量对象 / 数组给子组件
```jsx
// 错误（每次渲染都是新引用，memo 失效）
<Child info={{name:'张三'}} />

// 正确（用 useMemo 包一层）
const info = useMemo(() => ({name:'张三'}), []);
<Child info={info} />
```

### 5. 合理拆分组件
> 把不变的 UI 拆成独立子组件，不跟着父组件状态一起重渲染。

### 6. 能用 useRef 就别滥用 useState
> 不需要页面刷新的数据（定时器、DOM、临时变量）放 useRef。改 ref 不触发重渲染，改 state 必触发重渲染

# 二. 列表专项优化（后台管理 / 商品列表必用）
### 1. 列表一定要用稳定唯一 key
### 2. 长列表虚拟滚动
> 千条数据一次性渲染直接卡顿，用虚拟滚动只渲染可视区域企业常用库：react-virtualized、react-window
> 场景：订单列表、聊天记录、大数据表格
### 3. 列表分页 / 懒加载

# 三. 路由 & 组件懒加载（首屏提速）
### 1. React.lazy + Suspense 路由按需懒加载
> 不用一开始把所有页面都打包进首屏，切换路由才加载对应页面，首屏体积大幅变小。
```jsx
const Home = React.lazy(() => import('./Home'));

<Route path="/" element={
  <Suspense fallback={<div>加载中...</div>}>
    <Home />
  </Suspense>
}/>
```
### 2. 路由分包
> Vite/Webpack 自动把每个路由拆成独立 js 文件，按需加载。

# 四. 打包 & 构建优化（工程层面）
### 1. 第三方库拆分
> 大库（echarts、antd、lodash）单独拆分打包，不混业务代码。
### 2. CDN 引入大依赖
> react、react-dom、echarts 走 CDN，减少打包体积。
### 3. 开启 Tree-Shaking
> 删除项目未使用的冗余代码。
### 4. 图片 / 静态资源压缩
> 图片转 webp、压缩图标、静态资源托管 CDN。
### 5. 项目用 Vite 替代 Webpack
> 开发启动、热更新、打包速度直接翻倍。

# 五. 事件 & 请求优化
### 1. 防抖、节流
> 封装自定义 Hook useDebounce / useThrottle
### 2. 请求优化
> 重复请求做缓存，避免重复发接口
> 接口统一封装、取消重复请求（axios 取消令牌）
> 避免在 loop 里发请求
### 3. 避免在循环里 setState
> 会触发多次无用重渲染，统一合并更新。

# 六、DOM & 渲染层面优化
1. 减少多余 DOM 嵌套，别套无意义 div
2. 弹窗 / 浮层用 createPortal 传送门，避免父组件重渲染连带影响
3. 尽量用 class 切换样式，少频繁改行内 style
4. 避免频繁操作 DOM，优先用数据驱动视图

# 七、状态管理优化
> 如果用 Redux / Zustand / Pinia：
>> 精准订阅需要的状态，不要订阅整个全局状态
>> 拆分仓库，避免一个状态变化所有组件都刷新

# 优化优先级
1. 先解决 无脑重渲染（memo + useCallback + useMemo）
2. 长列表做 虚拟滚动 + 正确 key
3. 路由 懒加载分包 优化首屏
4. 加 防抖节流 + 请求缓存
5. 最后做 打包、图片、CDN 工程优化

# 总结
**React 性能优化常用**：
1. 用 memo/useCallback/useMemo 减少不必要重渲染；
2. 列表用唯一 key，大数据用虚拟滚动、分页；
3. 路由 lazy+Suspense 懒加载分包；
4. 防抖节流、请求缓存、避免循环 setState；
5. 工程层面拆包、CDN、资源压缩、Vite 构建；
6. 合理拆分组件、useRef 存非渲染变量。