# Vue

## Vue数据更新的方式

- 只要Vue中的数据发生改变, 那么 **页面** 中所有的指令和表达式, 都会被重新计算一次（事件对应的方法除外）
- 如果数据没有改变, 那么不会更新页面 如果数据发生了改变, 那么只会把这个数据对应页面的内容更新

## 过滤器 filter

- 作用：文本数据格式化 , 也就是: 将数据按照我们指定的一种格式输出
- 过滤器可以用在两个地方：`{{}}`和 v-bind 表达式
- 两种过滤器：1 全局过滤器 2 局部过滤器

### 全局过滤器

- 说明：通过全局方式创建的过滤器，在任何一个vue实例中都可以使用
- 注意：使用全局过滤器的时候，需要先创建全局过滤器，再创建Vue实例

```js
Vue.filter('filterName', function (value) {
  // value 表示要过滤的内容
  // 返回值：指定显示在页面中的内容
})
```

- 示例：

```html
<div>{{ dateStr | date }}</div>
<div>{{ dateStr | date('YYYY-MM-DD hh:mm:ss') }}</div>

<script>
  Vue.filter('date', function(value, format) {
    // value 要过滤的字符串内容，比如：dateStr
    // format 过滤器的参数，比如：'YYYY-MM-DD hh:mm:ss'
  })
</script>
```

### 局部过滤器

- 说明：局部过滤器是在某一个vue实例的内部创建的，只在当前实例中起作用

```js
{
  data: {},
  // 通过 filters 属性创建局部过滤器
  // 注意：此处为 filters
  filters: {
    filterName: function(value, format) {}
  }
}
```

## 键值修饰符

- 说明：在监听键盘事件时，Vue 允许为 `v-on` 在监听键盘事件时添加键值修饰符
- [键盘事件 - 键值修饰符](https://cn.vuejs.org/v2/guide/events.html#键值修饰符)
- 其他：修饰键（.ctrl等）、鼠标按键修饰符（.left等）

```js
// 只有在 keyCode 是 13 时调用 vm.submit()
@keyup.13 = "submit"
// 使用全局按键别名
@keyup.enter = "add"

---

// 通过全局 config.keyCodes 对象自定义键值修饰符别名
Vue.config.keyCodes.f2 = 113
// 使用自定义键值修饰符
@keyup.enter.f2 = "add"
```

## 监视数据变化 - watch

- 概述：`watch`是一个对象，键是需要观察的表达式，值是对应回调函数
- 作用：当表达式的值发生变化后，会调用对应的回调函数完成响应的监视操作
- [VUE $watch](https://cn.vuejs.org/v2/api/#vm-watch)

```js
new Vue({
  data: { a: 1, b: { age: 10 } },

  watch: {
    a: function(val, oldVal) {
      // val 表示当前值
      // oldVal 表示旧值
      console.log('当前值为：' + val, '旧值为：' + oldVal)
    },

    // 监听对象属性的变化
    // 注意：此时，val 和 oldVal 是相同的，都指向同一个对象
    b: {
      handler: function (val, oldVal) { /* ... */ },
      // 深度监听数据变化
      deep: true,
      // 立即触发监视
      immediate: true
    },

    // 只监视user对象中age属性的变化
    'user.age': function (val, oldVal) {
    },
  }
})
```

## 计算属性

- 说明：计算属性是基于它们的依赖进行缓存的，只有在它的依赖发生改变时才会重新求值
- 注意：Mustache语法（{{}}）中不要放入太多的逻辑，否则会让模板过重、难以理解和维护
- 注意：**`computed`中的属性不能与`data`中的属性同名，否则会报错**

- [Vue computed属性原理](http://www.cnblogs.com/kidney/p/7384835.html?utm_source=debugrun&utm_medium=referral)

```js
var vm = new Vue({
  el: '#app',
  data: {
    firstname: 'jack',
    lastname: 'rose'
  },
  computed: {
    fullname () {
      return this.firstname + '.' + this.lastname
    }
  }
})
```

## 实例生命周期

- 所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象即可 (一些根实例特有的选项除外)。
- 实例生命周期也叫做：组件生命周期

### 生命周期介绍

- [vue生命周期钩子函数](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)
- 简单说：**一个组件（实例）从开始到最后消亡所经历的各种状态，就是一个组件（实例）的生命周期**

生命周期钩子函数的定义：从组件被创建，到组件挂载到页面上运行，再到页面关闭组件被卸载，这三个阶段总是伴随着组件各种各样的事件，这些事件，统称为组件的生命周期函数！

开发人员可以通过Vue提供的钩子函数，让我们写的代码参与到Vue的生命周期里面来，让我们的代码在合适的阶段起到相应的作用。

- 注意：Vue在执行过程中会自动调用`生命周期钩子函数`，我们只需要提供这些钩子函数即可
- 注意：钩子函数的名称都是Vue中规定好的！

### 钩子函数 - beforeCreate()

- 不常用
- 说明：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用
- 注意：此时，无法获取 data中的数据、methods中的方法

### 钩子函数 - **created()**

- 注意：这是一个常用的钩子函数，可以调用methods中的方法、改变data中的数据
- [vue实例生命周期 参考1](https://segmentfault.com/a/1190000008879966)
- [vue实例生命周期 参考2](https://segmentfault.com/a/1190000008010666)
- 使用场景：发送请求获取数据

### 钩子函数 - beforeMounted()

- 说明：在挂载开始之前被调用

### 钩子函数 - **mounted()**

- 注意：这是一个常用的钩子函数
- 说明：此时，vue实例已经挂载到页面中，可以获取到el中的DOM元素，进行DOM操作

### 钩子函数 - beforeUpdated()

- 说明：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
- 注意：此处获取的数据是更新后的数据，但是获取页面中的DOM元素是更新之前的

### 钩子函数 - updated()

- 说明：组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。

### 钩子函数 - beforeDestroy()

- 说明：实例销毁之前调用。在这一步，实例仍然完全可用。
- 使用场景：实例销毁之前，执行清理任务，比如：清除定时器等

### 钩子函数 - destroyed()

- 说明：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

## axios

- Promise based HTTP client for the browser and node.js
  - 以Promise为基础的HTTP客户端，适用于：浏览器和node.js
  - 封装ajax，用来发送请求，异步获取数据
- 安装：`npm i -S axios`
- [axios 推荐](https://github.com/axios/axios)
- [vue-resource 原来推荐使用，现在不推荐](https://github.com/pagekit/vue-resource)

```js
// 在浏览器中使用，直接引入js文件使用下面的GET/POST请求方式即可
// 1 引入 axios.js
// 2 直接调用axios提供的API发送请求
created: function () {
  axios.get(url)
    .then(function(resp) {})
}

---
// 配合 webpack 使用方式如下：
import Vue from 'vue'
import axios from 'axios'
// 将 axios 添加到 Vue.prototype 中
Vue.prototype.$axios = axios

---
// 在组件中使用：
methods: {
  getData() {
    this.$axios.get('url')
      .then(res => {})
      .catch(err => {})
  }
}

---
// API使用方式：

axios.get(url[, config])
axios.post(url[, data[, config]])
axios(url[, config])
axios(config)
```

### Get 请求

```js
const url = 'http://localhost:8899/api/getnewslist'

// url中带有query参数
axios.get('/user?id=89')
  .then(function (response) {
    console.log(response)
  })
  .catch(function (error) {
    console.log(error)
  })

// url和参数分离，使用对象
axios.get('/user', {
  params: {
    id: 12345
  }
})

// axios() 直接发送get请求
axios({
  method: 'get',
  url: 'http://localhost:8899/api/getnewslist',
})
  .then(res => {
    console.log(res)
  })
```

### Post 请求

- [OPTIONS 预检请求](https://juejin.im/entry/58eaf351a22b9d0058a8e35c)
- [不同环境中处理 POST请求](https://github.com/axios/axios#using-applicationx-www-form-urlencoded-format)
- 默认情况下，axios 会将JS对象序列化为JSON对象（此时的 Content-Type为：application/json 而这个类型会导致预检请求）。为了使用 `application/x-www-form-urlencoded` 格式发送请求，我们可以这样：

```js
// 使用 qs 包，处理将对象序列化为字符串
// 注意：在浏览器环境中，需要通过引入 qs.js 以后，然后，通过 Qs 来使用~
// npm i -S qs
// var qs = require('qs')
import qs from 'qs'
axios.post('/foo', qs.stringify({ 'bar': 123 }))

// 或者：
axios.post('/foo', 'bar=123&age=19')
```

```js
const url = 'http://localhost:8899/api/postcomment/17'
axios.post(url, 'content=写的一手好段子')

axios.post('/user', qs.stringify({
    firstName: 'Fred',
    lastName: 'Flintstone'
  }))
  .then(function (response) {
    console.log(response)
  })
  .catch(function (error) {
    console.log(error)
  })

// axios() 直接发送post请求
axios({
  method: 'post',
  url: 'http://localhost:8899/api/postcomment/17',
  data: 'content=' + this.txt
})
  .then(res => {
    console.log(res)
  })
```

### 全局配置

```js
// 设置请求公共路径：
axios.defaults.baseURL = 'http://localhost:8899'
```

### 拦截器

- 拦截器会拦截发送的每一个请求，请求发送之前执行`request`中的函数，请求发送完成之后执行`response`中的函数

```js
// 请求拦截器
axios.interceptors.request.use(function (config) {
    // 所有请求之前都要执行的操作

    return config
  }, function (error) {
    // 错误处理

    return Promise.reject(error)
  })

// 响应拦截器
axios.interceptors.response.use(function (response) {
    // 所有请求完成后都要执行的操作

    return response
  }, function (error) {
    // 错误处理
    return Promise.reject(error)
  })
```

## 自定义指令

- 作用：进行DOM操作
- 使用场景：对纯 DOM 元素进行底层操作，比如：文本框获得焦点
- [vue 自定义指令用法实例](https://juejin.im/entry/58b7c5d8ac502e006cfee34a)
- 两种指令：1 全局指令 2 局部指令

### 全局指令

```js
// 第一个参数：指令名称
// 第二个参数：配置对象，指定指令的钩子函数
Vue.directive('directiveName', {
  bind() {},
  update() {}
})
Vue.directive('directiveName', function() {})

Vue.directive('red', function(el, binding) {
  el.style.color = binging.value
})
```

### 指令的钩子函数

- `bind` 和 `inserted` 的区别

```js
// DOM对象还没有插入到页面中
// bind中只能对元素自身进行DOM操作，而无法对父级元素操作
bind(el) {
  console.log(el.parentNode) // null
},
// inserted这个钩子函数调用的时候，也就是说可以获取到父级节点了
inserted(el) {
  console.log(el.parentNode) // <div id="app">...</div>
},
```

- `update` 和 `componentUpdated` 的区别

```js
// DOM重新渲染前
update(el) {
  console.log('update:', el.innerHTML)
},

// DOM重新渲染后
componentUpdated(el) {
  console.log('componentUpdated: ', el.innerHTML)
},
```

- 案例：1 模拟v-text 2 模拟v-on:click.prevent

### 局部指令

```js
{
  directives: {
    directiveName: {}
  }
}
```

```js
// 图片数据
list: [
  'http://www.benet-wh.com/uploads/allimg/170810/7-1FQ010502N92.gif',
  'http://img.mp.itc.cn/upload/20170227/3442e7d3e5834e34836e246ebe6dbabf.gif',
  'http://evgetgif.qiniudn.com/gifcoderlife-10.gif'
]
```

- [vue 剖析Vue原理&实现双向绑定MVVM](https://segmentfault.com/a/1190000006599500)
