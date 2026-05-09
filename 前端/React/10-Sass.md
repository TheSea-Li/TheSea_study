# 1. 什么是 Sass？
> Sass 是 CSS 预处理器，可以理解为：增强版、升级版 CSS。
> Sass 在 CSS 基础上增加了变量、嵌套、混合、继承、运算等高级语法，写完后自动编译成浏览器能识别的普通 CSS。
>> 区分：
>> Sass：老版本
>> SCSS：新版本，完全兼容原生 CSS，带大括号分号，现在所有人都用 SCSS。日常说的 Sass，默认就是指 SCSS，文件后缀 .scss

# 2. 如何使用
## 步骤1：安装
```bash
npm install sass
```
## 步骤2： 新建
直接新建xxx.scss文件，直接引入使用

# 3. 核心语法
## 1. 变量 $ （统一管理颜色 / 尺寸）
> 把常用颜色、字体、宽度定义成变量，改一处全局生效。
```scss
// 定义变量
$primary-color: #1677ff;
$font-size-base: 14px;
$border-radius: 8px;

.box {
  color: $primary-color;
  font-size: $font-size-base;
  border-radius: $border-radius;
}
```

## 2. 嵌套语法（常用）
> 原生 CSS 要重复写选择器，Sass 可以直接嵌套，结构清晰。
```html
<div class="card">
  <h3 class="title">标题</h3>
  <p class="desc">描述</p>
</div>
```
```scss
.card {
  width: 300px;
  padding: 20px;

  // 直接嵌套子元素
  .title {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .desc {
    color: #666;
  }
}
```

## 3. 父选择器 & （ hover / 伪类必备）
> & 代表当前父选择器，用来写悬浮、激活、同层级样式。
```scss
.btn {
  width: 100px;
  height: 36px;
  
  // & 等价于 .btn:hover
  &:hover {
    background: #1677ff;
    color: #fff;
  }

  &:active {
    opacity: 0.8;
  }
}
```

## 4. 混合器 @mixin / @include （复用大段样式）
> 把重复的一堆样式封装成混合器，哪里用哪里引入。
```scss
// 定义混合器
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

// 使用混合器
.wrap {
  width: 500px;
  height: 300px;
  @include flex-center; // 直接引入
}
```
还能传参数：
```scss
@mixin flex($dir: row) {
  display: flex;
  flex-direction: $dir;
}

.box {
  @include flex(column);
}
```

## 5. 支持运算
> 直接做加减乘除，不用自己算尺寸
```scss
$w: 200px;
.box {
  width: $w + 50px;  // 250px
  height: $w / 2;    // 100px
}
```

# 4. React 项目中实际用法
> 配合 CSS Modules 还能写成 xxx.module.scss
```jsx
import styles from './Card.module.scss';

function Card() {
  return <div className={styles.card}>Sass测试</div>;
}
```

# 5. Sass 优缺点
## 优点
- 支持变量、嵌套、混合，代码更简洁好维护
- 结构和 HTML 对应，可读性极强
- 样式可复用，减少冗余代码
- 完全兼容普通 CSS，老代码直接搬过来用
## 缺点
- 需要编译（项目已内置，无感）
- 新手要记一点语法
- 和 React 性能优化无关（只是写 CSS 更方便，不优化组件渲染）

# 6. 总结
Sass 就是带变量、嵌套、复用能力的增强版 CSS。可少写重复代码、样式更好维护、是 React/Vue 项目写传统样式的标配工具。