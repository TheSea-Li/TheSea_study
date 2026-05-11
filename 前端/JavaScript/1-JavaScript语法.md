# 一. 七大数据类型
## 1. 基本数据类型
> 存储在栈内存，赋值是拷贝一份独立值，互不影响。基本类型的值 直接存在栈内存的格子里，赋值时 = 把值复制一份，放到新格子里 → 两个变量完全独立，改谁都不影响谁。
```js
// 1. 声明变量a：栈内存开一个格子【a】，存入值 10
let a = 10;
// 2. 赋值：把a的值【10】拷贝一份，栈内存开新格子【b】，存入拷贝的10
let b = a; 
// 3. 修改a的值：只改【a】格子里的内容，变成20
a = 20;
// 结果：a和b完全独立，互不影响
console.log(a); // 20
console.log(b); // 10 
```

- String 字符串
- Number 数字（整数，小数）
- Boolean 布尔
- Null 空值
- Undefined 未定义
- Symbol 唯一标识
- BigInt 超大整数 

## 2. 引用数据类型
> 数据存在堆内存，栈里只存地址，赋值共用同一份数据，互相影响。
```js
let obj1 = { name: "张三" };
let obj2 = obj1;   // 拷贝地址，不是拷贝对象
obj2.name = "李四";
console.log(obj1.name); // 李四  互相影响
```

- Object 普通对象，数组，函数，正则，日期等

# 二. 栈内存 & 堆内存
## 1. 栈内存
存放：基本类型值、变量地址
特点：空间小、读取快、自动释放

## 2. 堆内存
存放：引用类型的真实数据（对象、数组内容）
特点：空间大、存放复杂数据

# 三. Math内置对象
## 1. 取整
```js
Math.floor(3.9);  // 向下取整 3
Math.ceil(3.1);   // 向上取整 4
Math.round(3.5);  // 四舍五入 4
```

## 2. 最值
```js
Math.max(10,20,30); // 最大值 30
Math.min(10,20,30); // 最小值 10
```

## 3. 随机数
```js
// 0 ~ 1 之间随机小数
Math.random();
// 取 1~10 随机整数
Math.floor(Math.random() * 10) + 1;
```

## 4. 其它常用
```
Math.abs(-5);    // 绝对值 5
Math.pow(2,3);   // 幂 2的3次方=8
Math.sqrt(9);    // 开平方 3
```

# 四. Date日期对象
## 1. 创建日期对象
```js
// 当前系统时间
let now = new Date();

// 指定时间
let d = new Date("2025-10-01");
```

## 2. 常用获取方法
```js
let d = new Date();

d.getFullYear();  // 年
d.getMonth();     // 月 0~11 要+1
d.getDate();      // 日
d.getHours();     // 时
d.getMinutes();   // 分
d.getSeconds();   // 秒

d.getTime();      // 获取时间戳（毫秒）
```

## 3. 时间戳
> 作用：用来比较时间先后、计算时间差。

# 五. 定时器
## 1. setTimeout 延时定时器
延迟指定时间，只执行一次
```js
// 2000毫秒后执行一次
let t1 = setTimeout(() => {
  console.log("延时2秒");
}, 2000);

// 清除定时器
clearTimeout(t1);
```

## 2. setInterval 循环定时器
每隔指定时间，重复执行
```js
// 每隔1秒执行一次
let t2 = setInterval(() => {
  console.log("每秒执行");
}, 1000);

// 停止循环
clearInterval(t2);
```

>注意：
>定时器时间单位：毫秒 1s = 1000ms
>定时器需要变量接收，才能手动清除
>setTimeout：可清除可不清除看情景。只执行一次，但关键是在它执行之前可能需要取消，如果不保存 ID，就无法提前中止。
>setInterval ：必须手动清除，否则它会无限循环执行，很容易导致页面卡顿或内存泄漏。

# 五. 数组高阶方法
> 放弃传统 for 循环，精通所有数组高阶方法，工作开发、逻辑处理全靠这些。
## 1. forEach 遍历
> 作用：纯粹遍历数组，无返回值，只做循环执行
> 特点：不返回新数组，不能中断遍历
```js
let arr = [1,2,3];
arr.forEach((item, index, arr) => {
  // item：当前每一项
  // index：下标
  // arr：原数组
  console.log(item, index);
});
```

## 2. map 映射（常用）
> 作用：遍历每一项，加工处理，返回一个新数组
> 特点：返回数组长度 和 原数组一致；不改变原数组
```js
let arr = [1,2,3];
// 每一项都乘2，返回新数组
let newArr = arr.map(item => item * 2);
console.log(newArr); // [2,4,6]
```

## 3. filter 过滤筛选
> 作用：按条件筛选，返回符合条件的新数组
> 特点：不改变原数组；只保留条件为 true 的项
```js
let arr = [10,20,30,40];
// 筛选大于20的数
let res = arr.filter(item => item > 20);
console.log(res); // [30,40]
```

## 4. find/findIndex
### find
> 作用：找到第一个符合条件的元素，找不到返回 undefined
```js
let arr = [10,20,30];
let item = arr.find(item => item === 20);
console.log(item); // 20
```

### findIndex
> 作用：找到第一个符合条件元素的下标，找不到返回 -1
```js
let index = arr.findIndex(item => item === 20);
console.log(index); // 1
```

## 5. some/every 判断
### some
> 作用：只要有一项满足条件，直接返回 true；全都不满足才 false
```js
let arr = [10,20,30];
let flag = arr.some(item => item > 25);
console.log(flag); // true
```

### every
> 作用：所有项都满足条件 才返回 true，有一个不满足就 false
```js
let flag = arr.every(item => item > 5);
console.log(flag); // true
```

## 6. reduce 汇聚（重点+难点）
> 作用：数组累加、求和、去重、数组转对象，万能汇聚方法
### 求和
```js
let arr = [1,2,3,4];
// sum：累加器，item：当前项
let total = arr.reduce((sum, item) => {
  return sum + item;
}, 0); // 初始值0
console.log(total); // 10
```

## 7. slice /splice
### slice 截取
> 作用：截取数组一部分，返回新数组，不改变原数组
```js
let arr = [1,2,3,4,5];
let res = arr.slice(1,3); // slice(开始下标, 结束下标) 包头不包尾
console.log(res); // [2,3]
```

### splice 删除/插入/替换
> 作用：删除、新增、替换数组元素，直接改变原数组
```js
let arr = [1,2,3,4];
// 从下标1开始，删除2个
arr.splice(1,2);
console.log(arr); // [1,4]
```

## 8. includes /indexOf 判断是否包含
### includes
> 作用：判断数组是否包含某一项，返回布尔值 true/false
```js
let arr = [10,20,30];
console.log(arr.includes(20)); // true
```

### indexOf
> 作用：找到元素下标，找到返回下标，找不到返回 -1
```js
console.log(arr.indexOf(20)); // 1
console.log(arr.indexOf(99)); // -1
```

## 9. 其它常用
### join 数组转字符串
```js
let arr = [1,2,3];
let str = arr.join('-'); 
console.log(str); // "1-2-3"
```

### sort 数组排序
```js
let arr = [3,1,2];
// 数字升序
arr.sort((a,b) => a - b);
// 数字降序
arr.sort((a,b) => b - a);
```






































