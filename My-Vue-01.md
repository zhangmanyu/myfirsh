#Vue
Vue是一个渐进式的javaScript框架

（数据驱动编程）（双向数据绑定）

1.在Vuue中，凡是v-开头的否叫做指令，指令是用来增强html功能用的

##MVVM
  M: Model（数据模型）
  V：View（视图模型）
  VM：（Vue）实列，连接数据


##插值表达式（展示数据）
  1.可以直接写变量
  2.可以使用字符串拼接
  3.可是数值运算
  4.可以写三元表达式
  注意：不能使用if else语句等JS相关的关键字
```html
  <h1>{{name}}<h1>
```
注意：该属性写在大括号里


##v-指令系类
###v-text
作用和差值表达式一样，也是用来展示数据的

  1.可以直接写变量
  2.可以使用字符串拼接
  3.可是数值运算
  4.可以写三元表达式
  注意：不能使用if else语句等JS相关的关键字

```html
  <h1 v-text="name"><h1>
```
注意：该属性写在属性里，用的少，了解即可

###v-html
写在元素属性位置，识别html标签
注意:此指令不安全，一般不使用


###v-bind指令
用于动态绑定属性,可以动态绑定任何属性

```html

全写方式：
<img  v-bind:src="url">
<!-- v-bind:属性名=“data的一个属性” -->
简写方式：
<img :src="url">
```

####动态绑定样式使用对象语法

```html
<p :class="{color:username==='jack'}"></p>
<!-- 对象语法：color是类名 ：后面是条件，当data中username等于18的时候才会动态添加上color类名 -->

<!-- 向后台传递数据的时候，识别变量可用es6中的字符串模板，或者字符串拼接 -->
<a :href="`app.html?id=${id}`">链接</a>
```



###v-for指令

用来实现遍历数组和对象

1. 遍历数组:
  1.1 v-for="item in arr"；
    item是一个参数，表示数组中的每一项
    arr也是一个参数，表示你要遍历的数组

  1.2. v-for="(item.index)in arr"；（掌握）
        index表示数组的索引，即多添加了一个参数

2. 遍历对象:
  2.1. v-for"calue in obj"
      value表示属性的值
      obj表示需要遍历的对象

  2.2. v-for="(value,key,index)       in obj";
        key表示属性建
        value表示属性值
        index表示属性的索引值

####以下两种方式不能动态刷新视图

  1. 使用数组的length属性去更改数组的时候
  2. 使用索引的方法去更改数组

  解决方法：
  1. Vue.set(arr,index,value)方法
  arr：表示需要设置的数组，
  index：表示数组索引，
  value：表示要设置或更改的值

  2. 直接调用数组的splice（）方法，进行查找替换

####key属性
用于提高循环渲染的效率
它的作用就是用来唯一标识数据的每一项，提高渲染的效率,可以让浏览器快速定位到我们要找到的数据。

```html
    <ul>
      <li v-for="(item,index) in list" :key="index">{{item.name}}</li>
    </ul>
```
注意：使用v-for循环渲染数据的时候，一点要记得将key属性加上去，key属性是唯一的标识，不能重复



###v-model
用来实现双向数据绑定
1. 双向数据绑定：就是数据模型中的数据与视图中的数据同步变化
2. 这个属性只能给input/select/textarea/组件这些标签使用



###v-on

监听DOM事件使用v-on指令 v-on：click=“函数”；

⭐简写：@事件类型=“函数”；

注意:事件类型可以自定义

####$event
如果项传递事件对象，就只能写$event,不能加引号


###v-show 和 v-if
  v-show：此方法是通过样式来控制元素的显示和隐藏

  v-if：此方法是通过操作DOM来控制元素的显示和隐藏

使用场景的区别：
 1. 如果页面设计到异步数据渲染的花，就用if
 2. 如果页面涉及到大量的DOM元素的显示和隐藏的话，就用show


###v-else
一看就懂

###v-else-if
一看就懂


###v-cloak
解决表达式闪烁问题，即文件加载完成时，渲染页面不知别双大括号，会闪一下大括号

使用方法：
  1. 找到闪烁的表达式标签，加上 v-cloak指令
  2. 使用属性选择器[v-cloak]设置样式display：none
  3. 当vue模板编译（加载）完成之后，会自动把标签上的v-cloak指令移除掉



##修饰符
###事件修饰符

  1. .stop:阻止事件冒泡
  2. .prevent:阻止默认行为


  ```html
  <a href="www.baidu.com" @click.pervrnt="函数：阻止跳转">打印阻止跳转</a>
  ```


###按键修饰符

```html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">



<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

      .enter
      .tab
      .delete (捕获“删除”和“退格”键)
      .esc
      .space
      .up
      .down
      .left
      .right



