# React样式的四种写法
> 2个基础规则：
> 1. React 里 不能用 class，要用 className
> 2. 标签里 不能用 for，要用 htmlFor

## 1. 内联样式（行内样式）
> 原生是 style="width:100px"React行内是双层大括号style={{}}。

**特点**
- 属性名改成驼峰：backgroundColor 不是 background-color
- 数字默认单位 px，可以不写
- 不能写伪类、动画、媒体查询

```jsx
// 示例
export default function Box() {
  return (
    <div style={{
      width: 200,
      height: 100,
      backgroundColor: 'skyblue',
      borderRadius: 8,
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'center'
    }}>
      行内样式
    </div>
  );
}
```
> 优点：快速、独立不污染
> 缺点：只能写简单样式，复杂样式写不了

## 2. 全局样式
> 写在index.css中

1. index.css文件
```css
/* 全局样式 */
.box {
  width: 200px;
  height: 100px;
  background: pink;
  border-radius: 8px;
  text-align: center;
  line-height: 100px;
}
```

2. 组件里引入＋使用
```jsx
import './index.css';

export default function Box() {
  return <div className="box">普通CSS样式</div>;
}
```
> 优点：写法和原生 CSS 一模一样，支持所有语法
> 缺点：全局污染，类名重名会互相覆盖，大项目不能用

## 3. CSS Modules
> 解决全局样式污染，组件样式局部生效，互不干扰。
>> 用法规则：
>> 1. 文件名必须格式：xxx.module.css
>> 2. 引入：import styles from './Box.module.css'
>> 3. 使用：className={styles.类名}

1. 新建Box.module.css文件
```css
.wrap {
  width: 200px;
  height: 100px;
  background: orange;
  border-radius: 8px;
  text-align: center;
  line-height: 100px;
}
```

2. 使用
```jsx
import styles from './Box.module.css';
。
export default function Box() {
  return <div className={styles.wrap}>CSS Modules 局部样式</div>;
}
```
> 优点：自动生成唯一类名，样式只作用当前组件。支持所有 CSS 语法（伪类、动画、媒体查询）。无命名冲突，中大型项目标配。
> 缺点：调用必须 styles.xxx，不能直接写字符串类名

## 4. Tailwind CSS
> 原子化CSS框架：把所有 CSS 都提前封装好成小工具类

1. 对比
普通写法：
```css
/* 要单独写 CSS */
.demo {
  width: 200px;
  height: 100px;
  background-color: skyblue;
  border-radius: 8px;
  text-align: center;
  line-height: 100px;
}
```
```jsx
<div className="demo">我是普通样式</div>
```
tailwind写法
```jsx
<div className="w-50 h-25 bg-sky-300 rounded-lg text-center leading-loose">
  Tailwind 直接拼样式
</div>
```
> 解析：
>> 尺寸： w-40 宽度  h-20 高度
>> 颜色：bg-red-500  背景红色  text-white 文字白色
>> 圆角、边距：rounded-lg 大圆角  m-4 外边距  p-4 内边距
>> 弹性布局：flex 弹性盒子  justify-center 水平居中  items-center 垂直居中

> 优点：不用手写CSS。不用起类名。开发速度极快。响应式布局超简单。适配 React/Vue 所有项目。
> 缺点：className 会很长（一堆类名堆一起）。新手要记一堆工具类（用多了自然就熟了）








