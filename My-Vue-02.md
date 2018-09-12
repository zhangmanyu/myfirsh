#Vue-02day

##ref属性
可以用来获取dom的引用。

##Vue.directive()
创建全局自定义指令，该方法有两个参数
1. 自定义指令名称，在元素中使用时需要加v-
2. 配置项（主要包含，配置项指令的函数），配置项中的函数都有两个参数：el：使用自定义指令的元素
binding:记录自定义指令信息的对象

注意：自定义指令名称不带 v-，建议全小写

```js
 // 利用Vue.directive()创建全局自定义指令，该方法有两个参数：一个自定义指令名称，一个是配置项(这里面主要包含一些和自定义指令执行相关的函数)
  // 1. 自定义指令名称 不带v-， 建议全部小写
  Vue.directive('myfocus', {
    // bind表示这个自定义指令一绑定到dom上，就开始自动执行
    bind(el,binding) {
      console.log('bind');
    },
    // 表示dom插入到页面上的时候自动执行
    // 这些函数都有两个参数，一个是el(使用自定义指令的元素), 一个是binding(记录自定义指令信息的对象)
    inserted(el, binding) {
      console.log('inserted');
      console.log(el);
      console.log(binding);
      el.focus()
    },
    // 表示自定义指令后面的值更新的时候，自动执行
    update() {
      console.log('update');
    }
  })
```


##过滤器

通过Vue.filter()方法，创建全局过滤器，用来将原数据过滤成想要的新数据





##computed ☆

用于计算属性，它表示根据已有属性计算得到一个新的属性。
计算属性是依赖缓存的，当页面中调用一个计算属性多次的时候，
不能处理异步数据




##watch 
会监听data中数据的变化，只要一变化，就执行相应的逻辑，监听的数据名放到里面做为函数名
可以处理异步事件的操作

```js
      // watch监听器会监听data中数据的变化，只要一变化，就能够执行相应的逻辑
        // 监听的数据名放到这里面作为函数名，这个函数里面有两个参数，一个是新值，一个是旧值
        watch: {
          firstName(newVal, oldVal) {
            console.log(newVal, oldVal)
            // 要用一个变量，将得到的数据保存起来
            this.fullName = newVal + this.lastName
          },
          lastName(newVal, oldVal) {
            setTimeout(() => {
              this.fullName = this.firstName + newVal
            }, 200);
          }
        }
        // 对比computed而言，这个computed性能更好，所以能用computed实现就用computed实现
        // 在涉及到异步数据操作的时候，就只能用watch去实现了。
```

注意点：
  1. 在监听复杂数据类型的时候不能像监听普通数据一样写，需要使用深度监听

```js

  var vm = new Vue({
        el: '#app',
        data: {
          user: {
            name: 'jack'
          }
        },
        watch: {
          // 在监听复杂数据类型的时候，不能像之前监听普通数据那样写，我们需要使用深度监听
          // user(newVal, oldVal) {
          //   console.log(newVal)
          // }
          user: {
            // 表示对象中属性变化的处理函数，这个函数只能叫这个名字
            handler(newVal) {
              console.log(newVal);
            },
            deep: true // 表示开启深度监听
          }
        }
      })
      
```