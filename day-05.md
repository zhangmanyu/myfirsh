# VUE

## Loaders（加载器）

- [webpack - Loaders](https://webpack.js.org/loaders/)
- [webpack - 管理资源示例](https://doc.webpack-china.org/guides/asset-management)

> webpack enables use of loaders to preprocess files. This allows you to bundle any static resource way beyond JavaScript.

- webpack只能处理JavaScript资源
- webpack通过loaders处理非JavaScript静态资源

## CSS打包

- 1 CSS打包文件（加载）
- 2 SASS打包文件（编译为CSS）

### 使用webpack打包CSS

- 安装：`npm i -D style-loader css-loader`
- 注意：use中模块的顺序不能颠倒，加载顺序：从右向左加载

```js
/* index.js */

// 导入 css 文件
import './css/app.css'


/* webpack.config.js */

// 配置各种资源文件的loader加载器
module: {
  // 配置匹配规则
  rules: [
    // test 用来配置匹配文件规则（正则）
    // use  是一个数组，按照从后往前的顺序执行加载
    {test: /\.css$/, use: ['style-loader', 'css-loader']},
  ]
}
```

### 使用webpack打包sass文件

- 安装：`npm i -D sass-loader node-sass`
- 注意：`sass-loader` 依赖于 `node-sass` 模块

```js
/* webpack.config.js */

// 参考：https://webpack.js.org/loaders/sass-loader/#examples
// "style-loader"  ：creates style nodes from JS strings 创建style标签
// "css-loader"    ：translates CSS into CommonJS 将css转化为CommonJS代码
// "sass-loader"   ：compiles Sass to CSS 将Sass编译为css

module:{
  rules:[
    {test: /\.(scss|sass)$/, use: ['style-loader', 'css-loader', 'sass-loader']},
  ]
}
```

## 图片和字体打包

- 安装：`npm i -D url-loader file-loader`
- `file-loader`：加载并重命名文件（图片、字体 等）
- `url-loader`：将图片或字体转化为base64编码格式的字符串，嵌入到样式文件中

```js
/* webpack.config.js */

module: {
  rules:[
    // 打包 图片文件
    { test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader' },

    // 打包 字体文件
    { test: /\.(woff|woff2|eot|ttf|otf)$/, use: 'file-loader' }
  ]
}
```

### 图片打包细节

- `limit`参数的作用：（单位为：字节(byte)）
  - 当图片文件大小（字节）`小于`指定的limit时，图片被转化为base64编码格式
  - 当图片文件大小（字节）`大于等于`指定的limit时，图片被重命名以url路径形式加载（此时，需要`file-loader`来加载图片）
- 图片文件重命名，保证相同文件不会被加载多次。例如：一张图片（a.jpg）拷贝一个副本（b.jpg），同时引入这两张图片，重命名后只会加载一次，因为这两张图片就是同一张

- 文件重命名以后，会通过MD5加密的方式，来计算这个文件的名称

```js
/* webpack.config.js */

module: {
  rules: [
    // {test: /\.(jpg|png|gif|jpeg)$/, use: 'url-loader?limit=100'},
    {
      test: /\.(jpg|png|gif|jpeg)$/,
      use: [
        {
          loader: 'url-loader',
          options: {
            limit: 8192
          }
        }
      ]
    }
  ]
}
```

### 字体文件打包说明

- 处理方式与图片相同，可以使用：`file-loader`或`url-loader`

## babel

- [babel](https://babeljs.io/)
- [es2015-loose](http://2ality.com/2015/12/babel6-loose-mode.html)
- [babel全家桶](https://github.com/brunoyang/blog/issues/20)
- 安装：`npm i -D babel-core babel-loader`
- 安装：`npm i -D babel-preset-env`

### 基本使用（两步）

- 第一步：

```js
/* webpack.config.js */

module: {
  rules: [
    // exclude 排除，不需要编译的目录，提高编译速度
    {test: /\.js$/, use: 'babel-loader', exclude: /node_modules/}
  ]
}
```

- 第二步：在项目根目录中新建`.babelrc`配置文件

```json
/* .babelrc */

// 将来babel-loader运行的时候，会检查这个配置文件，并读取相关的语法和插件配置
{
  "presets": ["env"]
}
```

## babel的说明

### babel的作用

- 1 语法转换：将新的ES语法转化为浏览器能识别的语法（babel-preset-*）
  - `var fn = () => {}`
- 2 transform-runtime 浏览器兼容：让低版本浏览器兼容最新版ES的API

### babel-preset-*

- 作用：将新的ES语法转化为浏览器能识别的ES5代码
  - 比如：`babel-preset-es2015`就是转化ES2015这个版本的JS语法
- [ES6语法提案的批准流程](http://es6.ruanyifeng.com/#docs/intro)
  - ES2015 也就是 ES6, 下一个版本是ES7, 从 ES6 到 ES7之间经历了 5 个阶段

```html
Stage 0 is "i've got a crazy idea",
stage 1 is "this idea might not be stupid",
stage 2 is "let's use polyfills and transpilers to play with it",
stage 3 is "let's let browsers implement it and see how it goes",
stage 4 is "now it's javascript".
```

### transform-runtime

- 作用：实现浏览器对不支持API的兼容（兼容旧环境、填补）
- [transform-runtime](https://babeljs.io/docs/plugins/transform-runtime/)
- 安装：`npm i -D babel-plugin-transform-runtime` 和 `npm i -S babel-runtime`
  - 注意：babel-runtime包中的代码会被打包到你的代码中（-S）

```js
/*
  transform-runtime 的使用步骤：

  1 在 .bablerc 中，添加如下配置即可
*/
"plugins": [
  "transform-runtime"
]
```

### 总结

```html
babel-core babel核心包

babel-loader 用来解析js文件

babel-preset-* 新ES语法的解析和转换

transform-runtime 兼容旧浏览器，到达支持新API目的
```

## vue单文件组件

- [vue-loader](https://vue-loader.vuejs.org/zh-cn/)
- single-file components(单文件组件)
- 后缀名：`.vue`，该文件需要被预编译后才能在浏览器中使用
- 注意：单文件组件依赖于两个包 **vue-loader** / **vue-template-compiler**
- 安装：`npm i -D vue-loader vue-template-compiler`

```html
<!-- App.vue 示例代码： -->
<template>
  <div>
    <h1>VUE 单文件组件示例 -- App.vue</h1>
    <p>这是 模板内容</p>
  </div>
</template>

<script>
  // 组件中的逻辑代码
  export default {}
</script>

<style>
/* 组件样式 */
h1 {
  color: red;
}
</style>
```

```js
// webpack.config.js 配置：
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module: {
  rules: [
    {
      test: /\.vue$/,
      loader: 'vue-loader'
    }
  ]
},

plugins: [
  new VueLoaderPlugin()
]
```

### 使用单文件组件

```js
/* main.js */

import Vue from 'vue'
// 导入 App 组件
import App from './App.vue'

const vm = new Vue({
  el: '#app',
  // 通过 render 方法，渲染App组件
  render: c => c(App)
})
```

### 单文件组件使用步骤

- 1 安装：`npm i -D vue-loader vue-template-compiler`
- 2 在 `webpack.config.js` 中配置 `.vue` 文件的loader
  - `{ test: /\.vue$/, use: 'vue-loader' }`
- 3 在 `webpack.config.js` 中添加 `plugins`：
  - `const VueLoaderPlugin = require('vue-loader/lib/plugin')`
  - `new VueLoaderPlugin()`
- 4 创建 `App.vue` 单文件组件，注意：App可以是任意名称
- 5 在 `main.js` 入口文件中，导入 `vue` 和 `App.vue`组件，通过 render 将组件与实例挂到一起

### 单文件组件+路由

- [vue - Vue.use](https://cn.vuejs.org/v2/api/#Vue-use)
- [Vue.use 和 路由](https://cn.vuejs.org/v2/guide/plugins.html#使用插件)

```js
import Vue from 'vue'
import App from './App.vue'

// ------------- vue路由配置 开始 --------------
import Home from './components/home/Home.vue'
import Login from './components/login/Login.vue'

// 1 导入 路由模块
import VueRouter from 'vue-router'
// 2 ** 调用use方法使用插件 **
Vue.use(VueRouter)
// 3 创建路由对象
const router = new VueRouter({
  routes: [
    { path: '/home', component: Home },
    { path: '/login', component: Login }
  ]
})

// ------------- vue路由配置 结束 --------------

const vm = new Vue({
  el: '#app',
  render: c => c(App),
  // 4 挂载到 vue 实例中
  router
})
```

## Mint-UI

- 基于 Vue.js 的移动端组件库
- [Mint-UI](http://mint-ui.github.io/#!/zh-cn)

### 快速开始

- 安装：`npm i -S mint-ui`

```js
// 1 导入 mint-ui模块
import MintUI from 'mint-ui'
// 2 导入 样式
import 'mint-ui/lib/style.css'
// 3 注册插件
Vue.use(MintUI)
```

## MUI

- [MUI](http://dev.dcloud.net.cn/mui/)
- MUI 也是移动端的UI库
- 使用：从github下载包，找到dist文件夹，只需要导入样式即可

```js
// 只需要导入 MUI的样式 即可，根据MUI的例子，直接使用HTML结果即可
// 导入样式
import './lib/mui/css/mui.min.css'
```

## ElementUI

- 这是PC端的UI组件库
- 安装：`npm i -S element-ui`
- [饿了吗 - ElementUI](http://element-cn.eleme.io/#/zh-CN/component/installation)

```js
{
  "presets": [
    ["es2015", { "modules": false }], "stage-0"
  ],

  "plugins": [
    ["component", [
      {
        "libraryName": "mint-ui",
        "style": true
      },
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-default"
      }
    ]]
  ]
}
```

## 脚手架

- [vue-cli github](https://github.com/vuejs/vue-cli/tree/master)
- 脚手架: 快速生成项目的目录结构
- 比如: vue的脚手架 `vue-cli`, 安装了vue的脚手架以后, 直接通过一条命令就可以生成一个vue项目的目录结构( webpack都配置好了 )

- 注意: **初始化脚手架的时候, 所在路径不能包含中文!!!!**

```html
vue脚手架的使用步骤:
1 全局安装 npm i -g vue-cli
2 通过vue命令初始化一个带有webpack模板的项目结构: vue init webpack my-project

我们使用脚手架构建vue项目的时候, 只需要修改 src目录 中的内容即可!!!

去公司以后, 拿到项目, 第一件事就是: npm i 安装项目中使用的模块
让项目跑起来: npm run dev / npm start
```

## vue 不同构建版本的说明（补充）

- [vue 版本](https://cn.vuejs.org/v2/guide/installation.html)
- `完整版`：同时包含编译器和运行时的版本
- `运行时（runtime）`：基本上就是除去编译器的其它一切
- 编译器：用来将模板字符串编译成为 JavaScript 渲染函数的代码

```js
// 导入 App 组件
import App from '../App.vue'

// 如果使用template，需要编译器，应该引入 完整版
new Vue({
  // 使用创建好的App组件作为模板内容
  template: '<App/>',
  // 创建局部组件
  components: { App }
})

// 如果使用render方法，不需要编译器，应该引入 运行时 版本
new Vue({
  render: c => c(App)
})
```

- 注意：`import Vue from 'vue'`默认导入 运行时 版本（vue.runtime.esm.js）
- 如果要导入完整版：
  - 1 在webpack中配置别名（alias）
  - 2 使用 `import Vue from 'vue'` 导入，此时导入的就是完整版

```js
resolve: {
  // 别名
  alias: {
    // 指定加载 vue 的完整版本
    // $表示精确匹配
    // https://doc.webpack-china.org/configuration/resolve/#resolve-alias
    'vue$': 'vue/dist/vue.esm.js',
  }
},
```
