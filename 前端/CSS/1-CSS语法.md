# 一. CSS 是什么
给 HTML 标签加样式（颜色、大小、位置、布局）。

# 二. CSS 三种引入方式
### 1. 内部样式（写在 html 里）
写在 <head> 的 <style> 标签里
```html
<head>
  <style>
    div {
      color: red;
    }
  </style>
</head>
```
### 2. 行内样式（内联样式）
优先级最高，适合临时改样式
```html
<div style="color: blue; font-size: 16px;">文字</div>
```

### 3.  外部样式
单独建 style.css 文件，用 link 引入
```html
<head>
  <link rel="stylesheet" href="style.css">
</head>
```

# 三. 三大基础选择器
### 1. 标签选择器
直接写标签名，选中所有同类标签
```css
div { color: red; }
```

### 2. 类选择器（开发最常用）
用 . 定义，标签用 class 引用，可以重复使用
```css
.box { font-size: 18px; }
```
```html
<div class="box">我是盒子</div>
```

### 3. id 选择器
用 # 定义，标签用 id，唯一不能重复。**一般留给 JS 获取元素**
```js
#app { margin: 0; }
```

# 四. 常用基础样式
```css
/* 文字样式 */
color: #333;          /* 文字颜色 */
font-size: 16px;       /* 字体大小 */
font-weight: bold;     /* 加粗 */
text-align: center;    /* 文字居中 */

/* 宽高 */
width: 200px;
height: 100px;

/* 背景 */
background-color: #eee;

/* 边框 */
border: 1px solid #ccc;

/* 外边距 上下左右 */
margin: 10px;
/* 内边距 */
padding: 10px;
```

# 五. 盒模型
**所有标签都是一个盒子**：
内容宽高content + 内边距 padding + 边框 border + 外边距 margin

# 六. Flex布局
1. **开启Flex**
给父元素加：
```css
.flex-box {
  display: flex;
}
```

2. ** 水平对齐**
```css
justify-content: center;     /* 水平居中 */
justify-content: space-between; /* 两边靠边，中间均分 */
```

3. ** 垂直对齐**
```css
align-items: center;  /* 垂直居中 */
```

4. ** 常用**
```css
.flex-box {
  display: flex;
  justify-content: center;
  align-items: center;
}
```













