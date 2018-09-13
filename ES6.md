# ES6 - ECMAScript 6.0（ES2015）

## 变量声明- let、const

### let

- ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

- `let`特点：
  - 1 块级作用域（ES6）
  - 2 先声明再使用
  - 3 不允许重复声明

```js
/* 基本使用 */
let num = 2

{
  let num = 9
}
console.log(num)
```

### const

- const声明一个**只读的常量**。一旦声明，常量的值就不能改变。
- const的作用域与let命令相同：只在声明所在的块级作用域内有效

```js
const PI = 3.1415
console.log(PI) // 3.1415

// 修改 常量的值 会报错
PI = 3 // TypeError: Assignment to constant variable.

// 可以修改对象中属性的值
const user = { name: 'rose' }
user.name = 'jack'
```

## 字符串模板

- 说明：代替原始的字符串拼接

```js
const num = 1

// ${} 中可以使用JS表达式
let dv = `<div>${num}</div>`
// 相当于:
let dv = `<div>` + num + '</div>'
```

## 箭头函数

- [ES6箭头函数](http://es6.ruanyifeng.com/#docs/function)
- 注意 1：函数体内的this对象，就是定义时所在的对象（一般是外层函数中的this）
- 注意 2：无法使用arguments，没有arguments对象
- 注意 3：不能当作构造函数，不能使用new创建对象
- 注意：**不要在Vue的选项属性或回调上使用箭头函数**
  - 比如：`created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`

```js
/* 语法： */
var fn = arg => arg

// 上面的箭头函数等同于：
var fn = function (arg) {
  return arg
}

var fn = () => {
  console.log('随机内容')
}
// 等同于：
var fn = function () {
  console.log('随机内容')
}
```

## rest参数

- ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了
- 说明：rest 参数的类型是：数组
- 注意：rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错

```js
function add(...values) {
  var sum = 0
  values.forEach(function(val) {
    sum += val
  })
  return sum
}

add(2, 5, 3) // 10


// 报错
function f(a, ...b, c) {
  // ...
}
```

## 解构赋值

- [ES6解构](http://es6.ruanyifeng.com/#docs/destructuring)
- ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

```js
// 对象解构
var { foo, bar } = { foo: "aaa", bar: "bbb" }
foo // "aaa"
bar // "bbb"

// 数组解构
var [a, b, c] = [1, 2, 3]
a // 1
b // 2
c // 3

// 函数参数的解构赋值
function foo({x, y}) {
  console.log(x, y) // 1 2
}

foo({x: 1, y: 2})
```

## 对象简化语法

- 对象中的属性和方法，都可以使用简化语法

```js
/* 属性的简化语法： */
var foo = 'bar'
var baz = {foo}

// 等同于
var baz = {foo: foo}


/* 方法的简化语法： */
var o = {
  method() {
    return "Hello!"
  }
}
// 等同于
var o = {
  method: function() {
    return "Hello!"
  }
}
```

### 属性名表达式

- ES6 允许字面量定义对象时，用表达式作为对象的属性名，即把表达式放在方括号内。

```js
var propKey = 'foo'
var methodKey = 'bar'

var obj = {
  [propKey]: true,
  // foo: true
  ['a' + 'bc']: 123,
  // abc: 123
  [methodKey] () {
    return 'hi'
  }
}
```

## class关键字

- ES6以前，JS是没有class概念的，而是通过构造函数+原型的方式来实现的
- 注意：ES6中的class仅仅是一个语法糖，并不是真正的类，与Java等服务端语言中的类是有区别的
- [ES6 - 文档](http://es6.ruanyifeng.com/#docs/class)

```js
class Person {
  constructor() {
    // 实例属性
    this.name = 'jack'
  }

  // 实例方法
  say() {}

  // 静态方法
  static coding() {}
}
// 静态属性
Person.age = 0

console.log(Person.age)
```

- 类继承：
  - 1 如果子类提供了 constructor，那么，必须要调用`super()`
  - 2 子类添加属性，必须在 super() 调用后面

```js
// 类继承：
class Chinese extends Person {
  constructor(name, gender, weight) {
    super(name, gender)

    this.weight = weight
  }
}

const ch = new Chinese('小明', '男', 130)
```

### 静态属性和实例属性

- 静态属性：直接通过类名访问
- 实例属性：通过实例对象访问

## ES6模块化 - import和export

- [导入和导出](http://blog.csdn.net/DeepLies/article/details/52916221?locationNum=13&fps=1)
- `import`：导入模块
- `export`：导出模块

- 注意1：`export default` 每个模块只能使用一次
- 注意2：`export` 每个模块可以使用多次
- 注意3：一个模块可以导出多个内容，`export default` 和 `export` 可以一起使用

```js
// main.js
// 导入 default 内容，可自定义导入名称
// import num from './a.js'
import num1 from './a.js'

// a.js
const num = 123
export default num
```

```js
// main.js
// 导入 export内容
// 注意：导入非default模块内容（str、fn），必须与 导出名称 相同，或者通过 as 修改
// 注意：必须使用花括号
import { str, fn } from './b'

// 加载并修改变量名
// import { str as str1, fn } from './b'
// 整体加载
// import * as bModule from './b'

// b.js
export const str = 'abc'
export function fn() {}
```

```js
// main.js
import { str, fn } from './b'

// b.js
const str = 'abc'
function fn() {}
// 一次性导出
export { str, fn }
```

## 数组扩展运算符

- 扩展运算符（spread）是三个点（...）。作用：将一个数组转为用逗号分隔的参数序列

```js
var arr = ['a', 'b', 'c']
console.log(...arr)

// 上面这句代码相当于：
console.log(arr[0], arr[1], arr[2])
```

## 对象扩展运算符

- 注意：该语法不是真正的ES规范，需要使用`stage-2`解析

```js
var obj = { name: 'jack', age: 19 }
var o = { ...obj, gender: 'male' }
// o => {name: 'jack', age: 19, gender: 'male'}
```

## Promise 异步编程

- Promise 是`异步编程`的一种解决方案，它允许你以一种同步的方式编写异步代码
- `promise`：承诺、保证
- [ES6 - Promise](http://es6.ruanyifeng.com/#docs/promise)

### 回调地狱问题

- JS是通过回调函数来实现异步编程的，当异步操作多了以后，就会产生回调嵌套回调的问题，这就是`回调地狱`

```js
// 按序读取文件：a -> b -> c -> d

readFileAsync('a.txt', (err, data) => {
  console.log('文件a: ', data)
  readFileAsync('b.txt', (err, data) => {
    console.log('文件b: ', data)
    readFileAsync('c.txt', (err, data) => {
      console.log('文件c: ', data)
      readFileAsync('d.txt', (err, data) => {
        console.log('文件d: ', data)
      })
    })
  })
})
```

- Promise方式：将异步操作以同步操作的方式表达出来，避免了层层嵌套的回调函数

```js
readFileAsync('a.txt')
  .then((data) => {
    console.log('文件a: ', data)
    return readFileAsync('b.txt')
  })
  .then((data) => {
    console.log('文件b: ', data)
    return readFileAsync('c.txt')
  })
  .then((data) => {
    console.log('文件c: ', data)
    return readFileAsync('d.txt')
  })
  .then((data) => {
    console.log('文件d: ', data)
  })
```

```js
// 终极形态：
async function fn () {
  const fileA = await readFileAsync('a.txt')
  console.log(fileA)
  const fileB = await readFileAsync('b.txt')
  console.log(fileB)
  const fileC = await readFileAsync('c.txt')
  console.log(fileC)
  const fileD = await readFileAsync('d.txt')
  console.log(fileD)
}

fn()
```

### 三种状态

- Promise对象代表一个异步操作，有三种状态：**pending（进行中）**、**fulfilled（已成功）**和 **rejected（已失败）**
  - 状态改变 1：pending -> fufilled
  - 状态改变 2：pending -> rejected
  - **一旦状态改变，就不会再变**

### 基本使用

- Promise构造函数：就是一个容器，用来封装异步操作

```js
// Promise 是一个构造函数
// 通过 new 创建Promise的实例对象
const p = new Promise(function(resolve, reject) {
  // resolve 表示成功，异步操作成功调用
  // reject  表示失败，异步操作失败调用
  fs.readFile('a.txt', (err, data) => {
    if (err) {
      // 文件读取失败，调用 reject
      reject(err)
      return
    }

    // 文件读取成功，调用 resolve
    resolve(data)
  })
})
```

### then 和 catch

- 说明：获取异步操作的结果
- `then()` ：用于获取异步操作成功时的结果 -> *resolve*
- `catch()`：用于获取异步操作失败时的结果 -> *reject*
- 说明：`then()`方法可以有多个，按照先后顺序执行，通过回调函数返回值传递数据给下一个then

```js
p
  // 成功
  .then((value) => {
    console.log('文件a的内容为：', value)
  })
  // 失败（比如：文件路径错误）
  .catch((err) => {
    console.log('文件读取失败：', err)
  })

// ----------- 或者 -----------
p
  .then((value) => {
    // 成功
  }, (err) => {
    // 失败
  })
```

### all 和 race

```js
// 所有请求发送成功：
const p = Promise.all([
  axios('http://vue.studyit.io/api/getlunbo'),
  axios('http://vue.studyit.io/api/getnewslist')
])

p.then(function (res) {
  // res 是 all() 方法中所有异步操作的结果
  console.log('两个异步请求完成：', res)
})

// 哪个请求先发送成功：
const p = Promise.race([
  axios('http://vue.studyit.io/api/getlunbo'),
  axios('http://vue.studyit.io/api/getnewslist')
])

p.then(function (res) {
  // res 是 race() 方法中先完成的异步操作的结果：
  console.log('一个异步请求完成：', res)
})
```

### Promise 的实现方式

- [q](https://github.com/kriskowal/q)
- [bluebird](https://github.com/petkaantonov/bluebird)
- [co](https://github.com/tj/co)
- [when](https://github.com/cujojs/when)

## async 和 await

- 异步编程终极方案
- 注意：`await`只能在`async`函数中使用
- 注意：`await`后面是一个`Promise`实例对象
- 注意：`await`关键字用来`暂停`后面的函数，等到获取到结果后，下面的代码才会执行
  - 这样，就可以按照代码书写的顺序来理解代码执行顺序
- [使用ES2017的Async功能](https://mp.weixin.qq.com/s/igjA_nY_l33gDArPfudwyw)
- [async/await替代Promise的6个理由](https://blog.fundebug.com/2017/04/04/nodejs-async-await/)

```js
// 封装 Promise函数
function timeout(ms) {
  return new Promise(resolve => {
    // setTimeout(resolve, ms, 'await')
    setTimeout((data) => {
      resolve(data)
    }, ms, '定时器')
  })
}

// 使用 async关键字 创建一个异步函数
async function fn() {
  console.log('await 前面')

  // 使用await关键字暂停函数，等待 Promise 的结果
  const ret = await timeout(3000)
  console.log(ret)

  console.log('await 后面')
}

fn()
```

## Promise 执行顺序问题

```js
let promise = new Promise(function(resolve, reject) {
  console.log('1 Promise')
  // 异步操作
  setTimeout(resolve, 1000, 'done')
})

promise.then(function() {
  console.log('3 resolved.')
})

console.log('2 Hi!')
```

## webpack 4.0 使用

- [webpack 4.0](https://jdc.jd.com/archives/211981)
- [webpack 4 测试版 —— 现在让我们先一睹为快吧！](https://juejin.im/post/5a72d569f265da3e3a6e2118)
- 1 `npm i -D webpack webpack-cli`
- 2 `webpack --mode development ./src/main.js`

```html
使用上的变化：
1 需要单独安装 webpack-cli 才能使用 webpack命令
2 使用webpack命令需要指定模式（development 或者 production）
3 默认的入口为：./src/index.js  默认出口为：./dist
```

### 说明

- 1 `webpack`的默认入口为：`/src/index.js`
- 2 webpack两种模式（`mode`）
  - 开发模式：`development`
  - 生成模式：`production` 压缩版
- 指定入口：`webpack ./src/main.js`
- 指定出口：`webpack ./src/main.js -o ./dist/bundle.js`
