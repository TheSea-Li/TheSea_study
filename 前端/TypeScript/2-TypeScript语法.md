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


























