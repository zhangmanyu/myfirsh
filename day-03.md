# Vue 强大的组件

## 组件

> 组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树

- ![组件化图释](./imgs/components.png)

- 创建组件的两种方式：1 全局组件 2 局部组件
- Vue实例中的配置项（如：methods、filters、watch、computed、directives、生命周期钩子函数）都可以在组件中使用

### 全局组件

- 说明：全局组件在所有的vue实例中都可以使用
- 注意：**先注册组件，再初始化根实例**
- 注意：组件中的 `data` 必须是函数

```js
// 1 注册全局组件
Vue.component('my-component', {
  template: '<p>A custom component!</p>',
  data() {
    return {
      msg: '注意：组件的data必须是一个函数！！！'
    }
  }
})

// 2 使用：以自定义元素的方式
<div id="example">
  <my-component></my-component>
</div>

// =====> 渲染结果
<div id="example">
  <p>A custom component!</p>
</div>
```

- template属性的值可以是：
  - 1 模板字符串
  - 2 模板id

```html
<!-- 2 模板id 示例 -->
<script type="text/x-template" id="tpl">
  <p>A custom component!</p>
</script>

template: '#tpl'
```

- `extend`：使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。

```js
// 注册组件，传入一个扩展过的构造器
Vue.component('my-component', Vue.extend({ /* ... */ }))

// 注册组件，传入一个选项对象 (自动调用 Vue.extend)
Vue.component('my-component', { /* ... */ })

// 内部执行过程：
const Home = Vue.extend({
  template: '',
  data() {}
})
Vue.component('home', Home)
```

### 局部组件

- 说明：局部组件，是在某一个具体的vue实例（组件）中定义的，只能在这个vue实例（组件）中使用

```js
const Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // 注意：此处为 components
  components: {
    // <my-component> 将只在当前vue实例中使用
    'my-component': Child
  }
})
```

## 组件通讯

- ![父子组件通讯示意图](./imgs/props-events.png)

### 父组件到子组件

- 方式：通过`props`属性来传递数据
- 注意：属性的值必须在组件中通过`props`属性显示指定，否则，不会生效
- 说明：传递过来的`props`属性的用法与`data`属性的用法相同

```html
<hello msg="120"></hello>
<hello my-msg="'abc'"></hello>

<!-- js -->
<script>
  components: {
    hello: {
      // 显式创建props及其传递过来的属性
      props: ['msg', 'myMsg'],
      template: '<h1>这是 hello 组件，这是消息：{{msg}} --- {{myMsg}}</h1>'
    }
  }
</script>
```

### 子组件到父组件

- 方式：父组件给子组件传递一个函数，由子组件调用这个函数
- 说明：借助vue中的自定义事件（v-on:cunstomFn="fn"）
- `$emit()`：触发事件

```html
<hello @pfn="parentFn"></hello>

<script>
  new Vue({
    methods: {
      // 父组件：提供方法
      parentFn(data) {
        console.log('父组件：', data)
      }
    }
  })

  Vue.component('hello', {
    template: '<button @click="fn">按钮</button>',
    methods: {
      // 子组件：通过$emit调用
      fn() {
        this.$emit('pfn', '这是子组件传递给父组件的数据')
      }
    }
  })
</script>
```

### 非父子组件通讯

> 在简单的场景下，可以使用一个空的 Vue 实例作为事件总线
- `$on()`：绑定事件

```js
const bus = new Vue()

// 触发组件 A 中的事件
bus.$emit('id-selected', 1)

// 在组件 B 创建的钩子中监听事件
bus.$on('id-selected', function (id) {
  // ...
})
```

- 示例：组件A ---> 组件B

```html
<!-- 组件A： -->
<com-a></com-a>
<!-- 组件B： -->
<com-b></com-b>

<script>
  const bus = new Vue()

  const vm = new Vue({
    el: '#app',
    components: {
      comB: {
        template: '<p>组件A告诉我：{{msg}}</p>',
        data() {
          return {
            msg: ''
          }
        },
        created() {
          // 定义事件：
          bus.$on('tellComB', (msg) => {
            this.msg = msg
          })
        }
      },

      comA: {
        template: '<button @click="emitFn">告诉B</button>',
        methods: {
          emitFn() {
            // 调用组件B中定义的事件：
            bus.$emit('tellComB', '地瓜地瓜我是土豆')
          }
        }
      }
    }
  })
</script>
```

### 获取组件（或元素） - refs

- 说明：`vm.$refs` 一个对象，持有已注册过 ref 的所有子组件（或HTML元素）
- 使用：在 HTML元素 中，添加`ref`属性，然后在JS中通过`vm.$refs.属性`来获取
- 注意：如果获取的是一个子组件，那么通过ref就能获取到子组件中的data和methods
- 在使用第三方的组件，可能会用到这个功能

```html
<div ref="dv"></div>

<!-- js -->
<script>
  vm.$refs.dv
</script>
```

## 案例：评论列表

## 组件的重点内容

- 1 如何创建组件（两种方式）
- 2 组件之间的通讯（父 -> 子， 子 -> 父， 非父子组件）
