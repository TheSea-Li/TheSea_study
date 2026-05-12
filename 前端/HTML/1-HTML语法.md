# 一. HTML标准骨架
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <!-- 编码 -->
  <meta charset="UTF-8">
  <!-- 移动端适配 必加 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>页面标题</title>
</head>
<body>
  <!-- 页面所有内容写在这里 -->
</body>
</html>
```
- head：放配置、标题、引入样式
- body：页面看得见的内容都放这

# 二. 块级元素 / 行内元素
### 1. 块级元素
- 独占一行，自动换行
- 可以设置宽高、边距

> **常见**：
> div h1~h6 p ul li

### 2. 行内元素
- 同行排列，不换行
- 不能设置宽高

> **常见**：
> span a img

# 三. 常用标签
### 1. 布局容器
- <div>：万能盒子，做布局、装内容，用得最多
- <header> 头部、<footer> 底部、<section> 区块：语义化标签（用法和 div 一模一样，只是更规范）

### 2. 文本标签
- <h1>~<h6>：标题，h1 最大，h6 最小
- <p>：段落
- <span>：行内文字，用来局部改样式
- <br>：强制换行
- <hr>：水平分割线

### 3. 超链接 a
```html
<!-- 跳转网页 -->
<a href="https://www.baidu.com">去百度</a>
<!-- 新标签页打开 -->
<a href="地址" target="_blank">新页面打开</a>
```

### 4. 图片 img
```html
<!-- src图片地址，alt图片加载失败显示文字 -->
<img src="图片路径" alt="图片描述">
```

### 5. 列表
- **无序列表（最常用）**
```html
<ul>
  <li>列表项1</li>
  <li>列表项2</li>
</ul>
```

- **有序列表（自带1，2，3编号）**
```html
<ol>
  <li>第一项</li>
</ol>
```

- **表单标签（重点！和 JS 交互必用）**
```html
<!-- 输入框 -->
<input type="text" placeholder="请输入用户名">

<!-- 密码框 -->
<input type="password" placeholder="请输入密码">

<!-- 单选框 -->
<input type="radio">

<!-- 复选框 -->
<input type="checkbox">

<!-- 多行文本域 -->
<textarea></textarea>

<!-- 按钮 -->
<button>登录</button>
```

# 四. 通用核心属性（所有标签都能用）
- **class="box"**：给标签起类名，专门给 CSS 选样式用
- **id="box"**：唯一标识，专门给 JS 获取元素用
- **data-id="100"**：自定义属性，JS 常用来存 id、数据
- **title="提示文字"**：鼠标悬浮上去显示提示




























