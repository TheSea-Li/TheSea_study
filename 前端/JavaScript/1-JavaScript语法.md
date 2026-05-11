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

# 六. 数组高阶方法
放弃传统 for 循环，精通所有数组高阶方法，工作开发、逻辑处理全靠这些。
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

# 七. let /const 块级作用域
> var 有变量提升、无块级作用域、可重复声明、容易污染全局。现在规范优先 const（常量，不修改），会变的变量用 let，永远不用 var。
### 1. 块级作用域
{ } 内部声明的 let/const，外面访问不到
```js
{
  let a = 10;
}
console.log(a); // 报错
```

### 2. 不能重复声明
```js
let num = 10;
let num = 20; // 报错
```

### 3. 暂时性死区
块内先使用再声明，直接报错，不会像 var 那样 undefined。

### 4. 使用规则
- 不变的值、对象、数组 → const
- 后续要重新赋值的普通变量 → let

# 八. 解构赋值
快速从数组、对象中快速取出值。
### 1. 数组结构
```js
let arr = [10, 20, 30];
// 解构
let [a, b, c] = arr;
console.log(a, b, c); // 10 20 30

// 跳过某一项
let [x, , z] = arr;

// 设置默认值
let [m = 1, n = 2] = [10];
```

### 2. 对象结构
```js
let obj = { name: "小明", age: 18 };

// 按属性名解构
let { name, age } = obj;

// 重命名 + 默认值
let { name: username, age = 20 } = obj;
```

# 九. 模板字符串 
代替双引号、单引号，支持换行、直接嵌入变量。
```js
let name = "小明";
let age = 18;

// 嵌入变量/表达式
let str = `我的名字是${name}，今年${age}岁`;

// 支持直接换行，不用拼接
let html = `
  <div>
    <p>文字</p>
  </div>
`;
```

# 十. 箭头函数
**简写规则**
- 省略 function
- 只有一个参数可省略括号
- 单行返回可省略 {} 和 return
```js
// 普通函数
function fn1(n) {
  return n * 2;
}

// 箭头函数完整版
let fn2 = (n) => {
  return n * 2;
};

// 极简写法
let fn3 = n => n * 2;
```
> 没有自己的 this，继承外层作用域的 this
> 不能作为构造函数（不能 new）
> 没有 arguments

# 十一. 扩展运算符 ... / 剩余运算符 ...
### 1. 拓展运算符
> 拆分数组 / 对象
```js
// 数组合并、拷贝
let arr1 = [1, 2];
let arr2 = [...arr1, 3, 4]; 

// 对象拷贝、合并
let obj1 = { a: 1 };
let obj2 = { ...obj1, b: 2 };
```

### 2. 剩余运算符
> 把剩余参数收拢成数组
```js
// 函数剩余参数
function sum(...args) {
  console.log(args); // [10,20,30]
}
sum(10, 20, 30);

// 解构剩余
let [a, ...rest] = [1,2,3,4];
// rest = [2,3,4]
```

# 十一. 对象简写 & 属性名表达式
### 1. 对象属性简写
变量名和属性名一致，可简写
```js
let name = "小明";
let age = 18;

// 简写
let obj = { name, age };
// 等价于 { name:name, age:age }
```

### 2.动态属性名
用 [] 写动态变量当属性名
```js
let key = "username";
let obj = {
  [key]: "小明"
};
```

# 十二. Set / Map 数据结构
### Set 集合
自动去重，值唯一
```js
// 数组去重
let arr = [1,1,2,2,3];
let s = new Set(arr);
// 转回数组
let newArr = [...s]; // [1,2,3]
```

###  Map 键值对
ES6 新增的两种内置数据结构，专门弥补数组、普通对象的短板：
### Set 集合
**特点**
- 成员自动去重，不允许重复值
- 有序：按插入顺序保存
- 可以存任意类型：数字、字符串、对象、数组等
- 没有下标，不能通过索引取值

**创建set**
```js
// 空Set
const s1 = new Set();

// 传入数组初始化，自动去重
const s2 = new Set([1,2,2,3,3,4]);
console.log(s2); // Set(4) {1,2,3,4}
```

**常用api**
方法 / 属性	作用
size	获取元素个数（类似数组 length）
add()	添加元素
delete()	删除指定元素
has()	判断是否存在某元素，返回布尔
clear()	清空所有元素
```js
const set = new Set();

set.add(1);
set.add(2);
set.add(1); // 重复，无效添加

console.log(set.size); // 2
console.log(set.has(2)); // true

set.delete(2);
console.log(set); // Set(1) {1}

set.clear();
```

**Set 经典用法：数组去重**
```js
const arr = [1,2,2,3,3,3];
// Set去重 再转回数组
const newArr = [...new Set(arr)];
console.log(newArr); // [1,2,3]
```

**遍历 Set**
```js
const set = new Set([1,2,3]);

// for...of
for (let item of set) {
  console.log(item);
}

// forEach
set.forEach(item => console.log(item));
```

### Map 键值对
**比较**
1. 普通对象 {} 痛点：
- 键只能是字符串 / Symbol
- 遍历不方便、无序
- 无法直接获取长度
2. Map 优势：
- 键可以是任意类型：数字、对象、数组、函数都行
- 按插入顺序有序
- 自带 size 直接获取键值对数量
- 遍历友好，增删性能更好

**创建Map**
```js
// 空Map
const m1 = new Map();

// 二维数组初始化
const m2 = new Map([
  ['name', '张三'],
  [18, '年龄'],
  [{id:1}, '对象当键']
]);
```

**常用api**
方法 / 属性	作用
size	获取键值对数量
set(key, val)	设置键值对
get(key)	通过键取值
has(key)	判断键是否存在
delete(key)	删除指定键
clear()	清空

```js
const map = new Map();

// 任意类型当key
let objKey = { id: 1 };
map.set('name', '李四');
map.set(100, '分数');
map.set(objKey, '我是对象键的值');

console.log(map.get('name')); // 李四
console.log(map.get(objKey)); // 我是对象键的值
console.log(map.has(100)); // true
console.log(map.size); // 3
```
**遍历Map**
```js
const map = new Map([['name','张三'],['age',20]]);

// 1. 遍历每一项 [key, value]
for (let [k, v] of map) {
  console.log(k, v);
}

// 2. 只遍历key
for (let k of map.keys()) {}

// 3. 只遍历value
for (let v of map.values()) {}

// 4. forEach
map.forEach((v, k) => console.log(k, v));
```


# 十三. JS 单线程 与 同步 / 异步
1. js特点
JavaScript 是单线程：同一时间只能做一件事。
2.同步
从上到下，按顺序执行，上一行没完，下一行等着。
```js
console.log(1);
console.log(2);
console.log(3);
// 输出 1 2 3 顺序固定
```
3.异步
代码不会等待，先往后执行，后面事情做完了再回头执行。
常见异步：
- 定时器 setTimeout
- 网络请求 / AJAX / Fetch
- 事件绑定

```js
console.log(1);

setTimeout(() => {
  console.log(2);
}, 0);

console.log(3);

// 输出顺序：1 → 3 → 2
```
>注意：定时器哪怕延时 0 毫秒，也是异步，会后执行。

# 十四. 回调函数
1. 什么是回调函数
把一个函数当做参数，传给另一个函数，将来再执行。
```js
function fn(cb) {
  console.log("执行任务");
  // 回头调用传进来的函数
  cb();
}

fn(() => {
  console.log("回调执行");
});
```

2. 回调地狱
多个异步操作依赖先后顺序，会一层套一层，代码向右无限嵌套，难维护、难读。
```js
setTimeout(() => {
  console.log("第一步");
  setTimeout(() => {
    console.log("第二步");
    setTimeout(() => {
      console.log("第三步");
    }, 1000);
  }, 1000);
}, 1000);
```
> Promise 就是为了解决回调地狱而生。

# 十五. Promise 核心
1. Promise 三种状态
- pending 进行中
- fulfilled 成功
- rejected 失败
状态一旦改变，就不能再变。

2. 基础语法
```js
const p = new Promise((resolve, reject) => {
  // 异步任务
  let flag = true;

  if (flag) {
    // 成功：触发 then
    resolve("成功结果");
  } else {
    // 失败：触发 catch
    reject("失败原因");
  }
});

// 接收结果
p.then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
}).finally(() => {
  console.log("无论成功失败都会执行");
});
```

3. Promise 链式调用
解决回调地狱，用 .then 竖着写，不嵌套
```js
new Promise((resolve) => resolve(1))
.then(res => {
  console.log(res);
  return 2;
})
.then(res => {
  console.log(res);
  return 3;
});
```

4. Promise 常用静态方法
- Promise.all()
所有都成功才成功，一个失败直接失败；适合并行请求。
- Promise.race()
谁先完成就用谁的结果，赛跑机制。
-Promise.allSettled()	
全部完成后返回每个结果的描述
- Promise.any	
第一个成功的 Promise 的结果
- Promise.resolve()
直接返回成功的 Promise
- Promise.reject()
直接返回失败的 Promise

# 十六. async /await
是 Promise 的语法糖，让异步代码写成同步写法，不用 .then 链式。
1. 基础用法
- async 修饰函数，函数内部可以用 await
- await 后面必须跟 Promise
- await 会等待异步完成，再往下执行
```js
// 封装一个Promise函数
function getData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("拿到数据");
    }, 1000);
  });
}

// async await 使用
async function fn() {
  console.log("开始");
  // 等待异步完成
  const res = await getData();
  console.log(res);
  console.log("结束");
}

fn();
```

2. 异常捕获
await 报错要用 try / catch
```js
async function fn() {
  try {
    const res = await getData();
    console.log(res);
  } catch (err) {
    console.log("出错了", err);
  }
}
```

3.串行 & 并行
- 多个 await 挨个写 → 串行（按顺序执行）
- 配合 Promise.all → 并行（同时执行）
```js
// 串行
// 多个独立的 await 会等前一个完成后再执行后一个，总耗时是所有任务耗时之和。
// 模拟异步任务：延迟返回，参数为耗时(ms)
function delay(ms, value) {
  return new Promise(resolve => setTimeout(() => resolve(value), ms));
}

(async () => {
  console.time('串行总耗时');

  const result1 = await delay(1000, '任务一完成');  // 等 1 秒
  const result2 = await delay(1000, '任务二完成');  // 再等 1 秒
  const result3 = await delay(1000, '任务三完成');  // 再等 1 秒

  console.log(result1, result2, result3);
  console.timeEnd('串行总耗时'); // 约 3000ms
})();
```

```js
// 并行
// 把所有异步任务同时启动，然后 await Promise.all([...]) 等它们都完成，总耗时为其中最慢的那个。
(async () => {
  console.time('并行总耗时');

  const p1 = delay(1000, '任务一完成');
  const p2 = delay(1000, '任务二完成');
  const p3 = delay(1000, '任务三完成');

  // 三个任务几乎同时启动，一起等待
  const [result1, result2, result3] = await Promise.all([p1, p2, p3]);

  console.log(result1, result2, result3);
  console.timeEnd('并行总耗时'); // 约 1000ms
})();
```

# 十七. 事件循环
- JS 执行顺序：同步代码 → 微任务 → 宏任务
- 宏任务：定时器、AJAX、事件
- 微任务：Promise.then/catch、async/await

# 十七. 原生 AJAX & Fetch 
1. 原生AJAX
- 创建 XMLHttpRequest 对象
- 初始化请求
- 监听状态变化
- 发送请求

2. Fetch
浏览器原生网络请求，基于 Promise：
```js
fetch("https://xxx.com/api")
.then(res => res.json())
.then(data => console.log(data))
.catch(err => console.log(err));
```











