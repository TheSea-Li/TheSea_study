# 一、类型注解
给变量指定固定类型，不能随便乱改类型。
```ts
// 语法：变量名: 类型 = 值
let name: string = "张三";
let age: number = 22;
let isLogin: boolean = true;

// 错误示范（TS直接报错）
age = "二十二"; 
// 数字类型不能赋值字符串
```
> 简写：赋值时可以自动推导类型，不用手动写
```ts
let name = "李四"; // TS自动识别为string
```

# 二、数组类型 两种写法
```ts
// 写法1：类型[]
let arr: number[] = [1,2,3];

// 写法2：Array<类型>
let list: Array<string> = ["a","b"];
```

# 三、函数类型（参数 + 返回值）
1. **指定参数类型、返回值类型**
```ts
// 参数加类型，括号后写返回值类型
function add(a: number, b: number): number {
  return a + b;
}

add(10, 20); // 正确
add(10, "20"); // 报错
```
2. **无返回值：void**
```ts
function logMsg(msg: string): void {
  console.log(msg);
}
```

3. **可选参数**
```ts
// b 可传可不传
function fn(a: number, b?: number): number {
  return a + (b || 0);
}
```

# 四、接口 interface（重中之重）
专门用来约束对象结构。规定对象必须有哪些属性、什么类型。
```ts
// 定义接口，规定对象结构
interface User {
  id: number;
  username: string;
  age?: number; // 可选属性
}

// 必须严格匹配接口结构
const u: User = {
  id: 1,
  username: "张三"
};
```

# 五、类型别名 type
给类型起个别名，简化代码，和 interface 很像
```ts
type NumStr = number | string;

let n: NumStr = 10;
n = "hello";
```

# 六、联合类型 |
一个变量可以是多种类型之一
```ts
let id: number | string;
id = 1001;
id = "1001";
```

# 七、简单泛型
复用类型，不写死，工作常用在封装工具函数、接口请求
```ts
// 1. 定义泛型函数：<T> 就是声明一个「类型占位符」，T 是 Type 的缩写（随便写，A/B/abc 都行，行业默认用 T）
function getData<T>(val: T): T {
  // val: T   → 参数类型是 T
  // ): T     → 返回值类型也是 T
  return val; 
}

// 2. 调用时：<number> 把 T 指定为 数字类型
getData<number>(10);

// 3. 调用时：<string> 把 T 指定为 字符串类型
getData<string>("你好");
```

# 八、类型断言 as
TS 识别不了类型的时候，强制指定。手动告诉 TS 变量的类型
```ts
// 比如获取DOM元素，TS默认是HTMLElement
const input = document.getElementById("input") as HTMLInputElement;
// 你知道它是输入框，断言成HTMLInputElement，才能拿到value
console.log(input.value);
```

# 九、交叉类型 &
把多个类型合并成一个，新类型拥有所有类型的属性
```ts
interface A { a: number }
interface B { b: string }

// 交叉类型，必须同时有a和b
type C = A & B;

const c: C = { a:1, b:"2" };
```

# 十、 枚举 enum
给一组固定不变的常量 起名字、分组。专门用来管理：状态、类型、性别、订单状态、菜单类型等固定值，让代码语义化、不写错、好维护。
### 1. 为什么要用枚举？
开发中经常遇到固定常量：
- 订单状态：0 = 待支付，1 = 已支付，2 = 已取消
- 请求状态：success = 成功，fail = 失败
- 性别：0 = 男，1 = 女

如果硬写数字 / 字符串（叫魔法值）：
```ts
// 看不懂 0 1 2 是什么
if(status === 0){}
// 容易拼错字符串，TS 不报错
if(res === 'succes'){} 
```
### 2. 两种最常用枚举
1. **数字枚举**
- 默认从 0 开始自动递增，也可以手动指定值
```ts
// 定义：订单状态枚举
enum OrderStatus {
  Pending,   // 0  待支付
  Paid,      // 1  已支付
  Canceled   // 2  已取消
}

// 使用
console.log(OrderStatus.Pending); // 0
console.log(OrderStatus.Paid);    // 1

// 业务判断
let status = 0;
if(status === OrderStatus.Pending){
  console.log("待支付");
}
```
- 手动指定起始值：
```ts
enum OrderStatus {
  Pending = 100, // 从100开始
  Paid,          // 101
  Canceled       // 102
}
```

2. **字符串枚举（开发首选）**
每个成员必须赋值字符串，可读性最高、无歧义
```ts
// 定义：请求状态枚举
enum RequestStatus {
  Success = "success",
  Fail = "fail",
  Loading = "loading"
}

// 使用
let res = "success";
if(res === RequestStatus.Success){
  console.log("请求成功");
}
```

# 十一、 类型守卫
联合类型下，判断变量具体是什么类型，让 TS 自动收窄类型
```ts
function fn(val: number | string) {
  // 类型守卫：判断是不是数字
  if (typeof val === "number") {
    // TS自动识别这里val是number
    val.toFixed(2);
  } else {
    // 这里val是string
    val.split("");
  }
}
```

# 十二、 泛型约束
给泛型加限制，不是任意类型都能传，必须满足某个条件
```ts
// 约束：T必须有length属性
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(val: T) {
  console.log(val.length);
}

logLength("abc"); // 字符串有length，没问题
logLength(123);   // 数字没有length，报错
```

# 十三、 类型推断
TS 会自动推导类型，很多时候不用手动写
```
let a = 10; // TS自动推导出a是number
function add(a:number,b:number) {
  return a+b;
}
// TS自动推导出返回值是number
```

# 十四、 元组（Tuple）
TS 特有的「固定长度 + 固定类型」的数组，常用于函数返回值、多值场景（比如坐标、键值对），比普通数组
```ts
// 语法：[类型1, 类型2, ...]
let point: [number, number] = [100, 200]; // 二维坐标（固定2个数字）
let userInfo: [string, number] = ["张三", 22]; // 姓名+年龄

// 越界赋值报错
point.push(300); // 虽然能push（历史兼容），但访问 point[2] 会提示类型错误
// 解构也能精准推导类型
const [x, y] = point; // x: number，y: number
```

# 十五、可选链操作符（?.）
处理嵌套对象的可选属性 / 可能为 null/undefined 的值，避免 Cannot read property 'xxx' of undefined 报错，TS 中高频使用。
```ts
interface User {
  info?: { // 可选属性
    address?: string;
  };
}

const u: User = {};
// 不用可选链：需要层层判断，代码冗余
console.log(u.info && u.info.address); 
// 用可选链：简洁，不存在则返回 undefined
console.log(u.info?.address); 

// 也可用于函数/数组
const fn: (() => number) | undefined = undefined;
fn?.(); // 函数不存在则不执行，无报错

const arr: number[] | undefined = undefined;
arr?.[0]; // 数组不存在则返回 undefined
```

# 十五、空值合并操作符（??）
解决 || 把 0、''、false 等「假值」误判为空的问题，仅对 null/undefined 生效，更精准。
```ts
// 需求：如果有值就用，无值（null/undefined）默认给 0
let num1 = 0;
console.log(num1 || 10); // 错误：0 被||判定为假，返回10
console.log(num1 ?? 10); // 正确：0 不是null/undefined，返回0

let num2: number | undefined = undefined;
console.log(num2 ?? 10); // 返回10
```

# 十六、接口 vs 类型别名（核心区别）
 type 和 interface 相似，其核心区别是面试 / 开发中高频问题
![type和interface区别]('../Images/TS_image2.png')
```ts
// 1. interface 可合并
interface User { id: number }
interface User { name: string }
// 最终 User = { id: number; name: string }

// 2. type 不可重复
type User = { id: number }
type User = { name: string } // 报错：标识符“User”重复

// 3. 拓展对比
// interface 继承
interface A { a: number }
interface B extends A { b: string }

// type 交叉
type A = { a: number }
type B = A & { b: string }
```

# 十七、非空断言（!）
手动告诉 TS：「这个值一定不是 null/undefined」，和 as 断言不同（as 是指定类型，! 是排除空值）
```ts
// 场景：TS 认为 input 可能为 null，但你确定 DOM 一定存在
const input = document.getElementById("input")!; 
console.log(input.value); // 不用写 as 也能直接用

// 函数参数非空断言
function fn(val: string | undefined) {
  console.log(val!.length); // ! 断言 val 不是 undefined
}
```

# 十八、never 类型
表示「永不存在的值的类型」，比如：抛出错误的函数、无限循环的函数、联合类型的「穷尽检查」。
```ts
// 1. 抛出错误的函数返回值
function throwError(msg: string): never {
  throw new Error(msg); // 永远不会执行到 return，返回 never
}

// 2. 穷尽检查（确保联合类型所有情况都被处理）
type Status = "success" | "fail";
function handleStatus(s: Status) {
  switch (s) {
    case "success": break;
    case "fail": break;
    default:
      // 如果漏写 case，这里会提示：s 是 never 类型（无剩余值）
      const _exhaustiveCheck: never = s;
      throw new Error(`未知状态：${_exhaustiveCheck}`);
  }
}
```

# 十九、readonly 只读修饰符
标记属性 / 数组为「只读」，防止意外修改，TS 编译时校验（编译后无限制）。
```ts
// 1. 接口/对象的只读属性
interface User {
  readonly id: number; // 只读，不可修改
  name: string;
}
const u: User = { id: 1, name: "张三" };
u.id = 2; // 报错：id 是只读属性

// 2. 只读数组
const arr: readonly number[] = [1,2,3];
arr.push(4); // 报错：只读数组无 push 方法
// 简写：ReadonlyArray<类型>
const list: ReadonlyArray<string> = ["a","b"];
```

# 二十、索引签名
```ts
// 需求：对象的键是任意字符串，值是数字（比如统计分数）
interface ScoreObj {
  [key: string]: number; // 索引签名：key 是 string 类型，值是 number
  math?: number; // 可加固定属性（类型要匹配）
}

const score: ScoreObj = {
  chinese: 90,
  math: 88,
  english: 95 // 任意键名，值必须是数字
};
```

# 二十一、函数重载
同一函数支持「不同参数类型 / 参数个数」的调用方式，让函数类型更精准（TS 编译后会忽略重载声明，仅保留实现）。
```ts
// 1. 重载声明（定义不同的入参+返回值）
function add(a: number, b: number): number;
function add(a: string, b: string): string;

// 2. 函数实现（需兼容所有重载场景）
function add(a: any, b: any): any {
  return a + b;
}

// 调用：TS 会根据参数类型推导返回值
add(1, 2); // 返回 number
add("1", "2"); // 返回 string
add(1, "2"); // 报错：无匹配的重载
```

# 二十二、typeof 类型查询
从「变量 / 对象」上提取类型，减少重复定义，实现「类型和值联动」。
```ts
// 场景：已有一个对象，想复用它的类型
const user = { id: 1, name: "张三", age: 22 };
// 用 typeof 提取 user 的类型
type User = typeof user; // 等价于 { id: number; name: string; age: number }

const u: User = { id: 2, name: "李四", age: 23 }; // 精准匹配

// 也可提取函数类型
function add(a: number, b: number) { return a + b; }
type AddFn = typeof add; // (a: number, b: number) => number
```

# 二十三、keyof 类型操作符
提取「类型 / 接口」的所有键名，返回联合字面量类型，常用于泛型 + 对象操作。
```ts
interface User { id: number; name: string; age: number }
// keyof User → "id" | "name" | "age"
type UserKeys = keyof User; 

// 泛型+keyof 实现：安全访问对象属性
function getProp<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

const u: User = { id: 1, name: "张三", age: 22 };
getProp(u, "name"); // 正确，返回 string
getProp(u, "gender"); // 报错：gender 不是 User 的键
```
















































