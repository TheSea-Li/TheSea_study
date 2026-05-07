# 1.JSX语法规则
- 必须闭合标签
- class->className：因为class是js关键字，故用className替代
- for->htmlFor
- 大写开头组件
```bash
# 正确写法
<Card /> 

# 错误写法
<card />
```

- 注释写法：{/* 注释内容 */}
- 根元素：return只能有一个根节点
```bash
return (
  <>  {/* 碎片 Fragment，空标签，不渲染 DOM */}
    <h1>标题</h1>
    <p>段落</p>
  </>
);
```

