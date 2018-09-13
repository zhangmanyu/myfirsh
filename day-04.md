# Vue

## SPA -单页应用程序

- SPA： `Single Page Application`

```html
单页Web应用（single page application，SPA），就是只有一个Web页面的应用，
是加载单个HTML页面，并在用户与应用程序交互时动态更新该页面的Web应用程序。
```

- 单页面应用程序：
  - ![SPA](imgs/web-spa.png)

- 传统多页面应用程序：
  - ![传统web](imgs/web-traditional.png)

```html
对于传统的多页面应用程序来说, 每次请求服务器返回的都是一个完整的页面

对于单页应用程序来说, 只有第一次会加载页面, 以后的每次请求, 仅仅是获取必要的数据.
  然后, 由页面中js解析获取的数据, 展示在页面中
```

### 优势

- 1 减少了请求体积，加快页面响应速度，降低了对服务器的压力
- 2 更好的用户体验，让用户在web app感受native app的流畅

### 主要技术点

- 1 ajax
- 2 哈希值（锚点）的使用（window.location.hash #）
- 3 hashchange 事件

### 实现思路

- 监听锚点值变化的事件，根据不同的锚点值，请求相应的数据
- 1 锚点（#）原本用作页面内部进行跳转，定位并展示相应的内容
- 2 SPA中，锚点被用作请求不同资源的标识，请求数据并展示内容

```html
http-server 包的使用：
1 全局安装 npm i -g http-server
2 需要在哪个文件夹中开启服务器，就在哪个目录中执行： http-server 即可
```

## 路由

- 路由即：浏览器URL中的哈希值（# hash）与展示视图内容（template）之间的对应规则
- vue中的路由是：hash 和 component的对应关系，一个哈希值对应一个组件

```html
在 Web app 中，通过一个页面来展示和管理整个应用的功能。
SPA往往是功能复杂的应用，为了有效管理所有视图内容，前端路由 应运而生！
简单来说，路由就是一套映射规则（一对一的对应规则），由开发人员制定规则。
当URL中的哈希值（# hash）发生改变后，路由会根据制定好的规则，展示对应的视图内容
```

### 基本使用

- 安装：`npm i -S vue-router`

```html
<div id="app">
  <!-- 5 路由入口：链接导航 -->
  <router-link to="/home">首页</router-link>
  <router-link to="/login">登录</router-link>

  <!-- 6 路由出口：用来展示匹配路由视图内容 -->
  <router-view></router-view>
</div>

<!-- 1 导入 vue.js -->
<script src="./vue.js"></script>
<!-- 2 导入 路由文件 -->
<script src="./node_modules/vue-router/dist/vue-router.js"></script>
<script>
  // 3 创建两个组件
  const Home = Vue.component('home', {
    template: '<h1>这是 Home 组件</h1>'
  })
  const Login = Vue.component('login', {
    template: '<h1>这是 Login 组件</h1>'
  })

  // 4 创建路由对象
  const router = new VueRouter({
    routes: [
      { path: '/home', component: Home },
      { path: '/login', component: Login }
    ]
  })

  const vm = new Vue({
    el: '#app',
    // 不要忘记，将路由与vue实例关联到一起！
    router
  })
</script>
```

### 重定向

- 解释：将 `/` 重定向到 `/home`

```js
{ path: '/', redirect: '/home' }
```

### 路由导航高亮

- 说明：当前匹配的导航链接，会自动添加`router-link-exact-active router-link-active`类
- 通过配置项 `linkActiveClass: 'now'` 来修改默认的高亮类名

```js
const router = new VueRouter({
  routes: [],

  // 修改默认高亮的a标签的类名
  linkActiveClass: 'now'
})
```

### 路由参数

- 说明：我们经常需要把某种模式匹配到的所有路由，全都映射到同一个组件，此时，可以通过路由参数来处理
- 语法：`/user/:id`
- 使用：当匹配到一个路由时，参数值会被设置到 this.$route.params
- 其他：可以通过 `$route.query` 获取到 URL 中的查询字符串（queryString） 等

```js
// 链接：
<router-link to="/user/1001">用户 Jack</router-link>
<router-link to="/user/1002">用户 Rose</router-link>

// 路由：
{ path: '/user/:id', component: User }

// User组件：
const User = {
  template: `<div>User {{ $route.params.id }}</div>`
}
```

### 嵌套路由 - 子路由

- 路由是可以嵌套的，即：路由中又包含子路由
- 规则：父组件中包含 `router-view`，在路由规则中使用 `children` 配置

```js
// 父组件：
const User = Vue.component('user', {
  template: `
    <div class="user">
      <h2>用户中心</h2>
      <router-link to="/user/profile">个人资料</router-link>
      <router-link to="/user/posts">岗位</router-link>

      <!-- 子路由展示在此处 -->
      <router-view></router-view>
    </div>
    `
})

// 子组件：
const UserProfile = {
  template: '<h3>个人资料：张三</h3>'
}
const UserPosts = {
  template: '<h3>岗位：FE</h3>'
}

{ path: '/user', component: User,
  // 子路由配置：
  children: [
    {
      // 当 /user/profile 匹配成功，
      // UserProfile 会被渲染在 User 的 <router-view> 中
      path: 'profile',
      component: UserProfile
    },
    {
      // 当 /user/posts 匹配成功
      // UserPosts 会被渲染在 User 的 <router-view> 中
      path: 'posts',
      component: UserPosts
    }
  ]
}
```

## Webpack

![webpack](./imgs/webpack-1.png)

- [webpack 官网](http://webpack.github.io/)
- bundle `[ˈbʌndl]` 捆绑，收集，归拢，把…塞入

```html
1 webpack 将带有依赖项的各个模块打包处理后，变成了独立的浏览器能够识别的文件
2 webpack 合并以及解析带有依赖项的模块
```

### 概述

- webpack的两个特点：1 模块化 2 打包

> webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)  
> webpack 是一个**模块化方案（预编译）**  
> webpack获取具有依赖关系的模块，并生成表示这些模块的静态资源

- 四个核心概念：**入口(entry)**、**输出(output)**、**加载器loader**、**插件(plugins)**
- ![webpack 与 模块化](./imgs/webpack-module.jpg)

```html
模块化方案: webpack 和 requirejs（通过编写代码的方式将前端的功能，划分成独立的模块）

browserify 是与 webpack 相似的模块化打包工具

webpack 预编译 (在开发阶段通过webpack进行模块化处理, 最终项目上线, 就不在依赖于 webpack)
requirejs 线上的编译( 代码运行是需要依赖与 requirejs 的 )
```

### webpack起源

- webpack解决了现存模块打包器的两个痛点：
  - 1 **Code Spliting** - 代码分离
  - 2 **静态资源的模块化处理方案**

### webpack与模块

- [前端模块系统的演进](http://zhaoda.net/webpack-handbook/module-system.html)
- 在webpack看来：所有的**静态资源都是模块**
- webpack 模块能够识别以下等形式的模块之间的依赖：

- JS的模块化规范：
  - ES2015 `import` `export`
  - CommonJS `require()` `module.exports`
  - AMD `define` 和 `require`

- 非JS等静态资源：
  - css/sass/less 文件中的 `@import`
  - 图片连接，比如：样式 `url(...)` 或 HTML `<img src=...>`
  - 字体 等

### webpack文档和资源

- [webpack 中文网](https://doc.webpack-china.org/)
- [webpack 1.0](http://webpack.github.io/docs/what-is-webpack.html)
- [webpack 2.x+](https://webpack.js.org/)
- [入门Webpack，看这篇就够了](http://www.jianshu.com/p/42e11515c10f#)

---

## 安装webpack

- 全局安装：`npm i -g webpack`
  - 目的：在任何目录中通过CLI使用 `webpack` 这个命令
- 本地安装：`npm i -D webpack` （推荐！！！）
  - 目的：执行当前项目的构建

## webpack的基本使用

- 安装：`npm i -D webpack webpack-cli`
- webpack的两种使用方式：1 命令行 2 配置文件（`webpack.config.js`）

- 1 运行`webpack ./src/main.js -o ./dist/bundle.js`进行打包构建，语法是：`webpack 入口文件 输出文件`
- 2 注意：需要在页面中引入 输出文件 的路径（此步骤可通过配置webpack去掉）

### 命令行使用说明

- `package.json`中的`scripts`中可以存放一些bash命令，这些bash命令可以通过 `npm run 命令名称` 来执行
- 注意：npm在执行scripts中的命令的时候，是在电脑系统后台默认开启一个bash，将当前目录下的`./node_modules/.bin`这个文件夹临时加入了系统环境变量

- 使用方式：`npm run dev`

```json
"scripts": {
  "build": "webpack"
}
```

### 命令行方式演示 - 案例：隔行变色

```js
/*
  src/main.js
*/

// 1 导入 jQuery
import $ from 'jquery'
// 2 获取页面中的li元素
const $lis = $('#ulList').find('li')
// 3 隔行变色
// jQuery中的 filter() 方法用来过滤jquery对象
$lis.filter(':odd').css('background-color', '#def')
$lis.filter(':even').css('background-color', 'skyblue')
```

### 配置文件方式（推荐）

```js
/*
  webpack.config.js

  运行命令：webpack

  entry 入口的配置说明：
  https://doc.webpack-china.org/concepts/entry-points
*/

const path = require('path')
module.exports = {
  // 入口文件
  entry: path.join(__dirname, 'src/js/main.js'),

  // 输出文件
  output: {
    path: path.join(__dirname, './dist'),   // 输出文件的路径
    filename: 'bundle.js'                 // 输出文件的名称
  }
}
```

## webpack-dev-server

- 安装：`npm i -D webpack-dev-server`
- 作用：配合webpack，创建开发环境（启动服务器、监视文件变化、自动编译、刷新浏览器等），提高开发效率
- 注意：无法直接在终端中执行 `webpack-dev-server`，需要在 `package.json` 配置 `scripts` 后使用

### 使用说明

- 注意：`webpack-dev-server`将打包好的文件存储在内存中，提高编译和加载速度，效率更高
- 注意：输出的文件被放到项目根目录中
  - 命令行中的提示：`webpack output is served from /`
  - 在`index.html`页面中直接通过 `/bundle.js` 来引入内存中的文件

### 配置说明 - CLI配置

- `--contentBase` ：告诉服务器在哪个目录中提供服务（可以理解为：打开哪个目录中的index.html）
  - `--contentBase ./`：当前工作目录
  - `--contentBase ./src`：当前目录下的src文件夹
- `--open` ：自动打开浏览器
- `--port` ：端口号
- `--hot` ：热更新，只加载修改的文件(按需加载修改的内容)，而非全部加载

```js
/* package.json */
/* 运行命令：npm run dev */

{
  "scripts": {
    "dev": "webpack-dev-server --contentBase ./src --open --port 8888 --hot"
  }
}
```

### 配置说明 - webpack.config.js

```js
const webpack = require('webpack')

devServer: {
  // 服务器的根目录 Tell the server where to serve content from
  // https://webpack.js.org/configuration/dev-server/#devserver-contentbase
  contentBase: path.join(__dirname, './'),
  // 自动打开浏览器
  open: true,
  // 端口号
  port: 8888,

  // --------------- 1 热更新 -----------------
  hot: true
},

plugins: [
  // ---------------- 2 启用热更新插件 ----------------
  new webpack.HotModuleReplacementPlugin()
]
```

## html-webpack-plugin 插件

- 安装：`npm i -D html-webpack-plugin`
- 作用：根据模板，自动生成html页面
- 优势：页面存储在内存中，自动引入`bundle.js`、`css`等文件

```js
/* webpack.config.js */
const htmlWebpackPlugin = require('html-webpack-plugin')

// ...
plugins: [
  new htmlWebpackPlugin({
    // 模板页面路径
    template: path.join(__dirname, './index.html'),
    // 在内存中生成页面路径，默认值为：index.html
    // filename: 'index.html'
  })
]
```
