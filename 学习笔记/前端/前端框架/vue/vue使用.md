
指令:参数.修饰符
<form v-on:submit.prevent="onSubmit"></form>
1.数据渲染进DOM，绑定 DOM 文本到数据
<div id="app">
{{ message }}
</div>

var app = new Vue({
el: '#app',
data: {
message: 'Hello Vue!'
}
})

或者用v-bind:属性=“数据”
缩写：用:

v-if：绑定 DOM 结构到数据
<div id="app-3">
<p v-if="seen">现在你看到我了</p>
</div>
var app3 = new Vue({
el: '#app-3',
data: {
seen: true
}
})


v-for；


<div id="app-4">
<ol>
<li v-for="todo in todos">
{{ todo.text }}
</li>
</ol>
</div>
var app4 = new Vue({
el: '#app-4',
data: {
todos: [
{ text: '学习 JavaScript' },
{ text: '学习 Vue' },
{ text: '整个牛项目' }
]
}
})


处理用户输入：


v-on:click=“clickHandle”
缩写@click
点击后调用clickHandle方法
自己不需要操作dom，通过更新数据


v-model 双向绑定
<div id="app-6">
<p>{{ message }}</p>
<input v-model="message">
</div>
var app6 = new Vue({
el: '#app-6',
data: {
message: 'Hello Vue!'
}
})
部分指令修饰符


过滤器

v-once:你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定：
<span v-once>This will never change: {{ msg }}</span>

除了 data 属性， Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。例如：
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
// 这个回调将在 `vm.a` 改变后调用
})

计算属性：
计算属性是基于它们的依赖进行缓存的
如果你不希望有缓存，请用 method 替代。
计算属性
// ...
computed: {
fullName: {
// getter
get: function () {
return this.firstName + ' ' + this.lastName
},
// setter
set: function (newValue) {
var names = newValue.split(' ')
this.firstName = names[0]
this.lastName = names[names.length - 1]
}
}
}
// ...
watch 属性



声明式渲染：
<div id="app">
{{ message }}
</div>

var app = new Vue({
el: '#app',
data: {
message: 'Hello Vue!'
}
})

app.message = ’hello hello';



指令：
指令（Directives）是带有 v- 前缀的特殊属性。指令属性的值预期是单一 JavaScript 表达式（除了 v-for，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上
参数：
一些指令能接受一个“参数”，在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：
“将这个元素节点的 title 属性和 Vue 实例的 message 属性保持一致”。
<a v-bind:href="url"></a>
<a href=“{{url}}”></a>
v-on 指令，它用于监听 DOM 事件：
<a v-on:click="doSomething”></a>

修饰符：
例如.prevent
<form v-on:submit.prevent="onSubmit"></form>
缩写 v-on:  @click
过滤器：
<!-- in mustaches -->
{{ message | capitalize }}
<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>

Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用计算属性。

过滤器函数总接受表达式的值作为第一个参数。
{{topic.create_at | getLastTimeStr(true)}}
ture作为第二个参数传入函数
//topic.create_at作为第一个参数
getLastTimeStr (aa, bb) => {
….
}
也可以这样
getLastTimeStr(user.create_at, false)"
过滤器可以串联：


###插值
##文本
<span> Message:{{msg}}</span>

无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新。
<span v-once>This will never change: {{ msg }}</span>
通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定：

##纯html
<div v-html="rawHtml"></div>
双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html 指令：

##属性
<div v-bind:id="dynamicId"></div>
缩写:id == v-bind:id

这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
<button v-bind:disabled="someDynamicCondition">Button</button>

##使用js表达式
对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。注意是表达式不是语句

模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。



###计算属性

在模板中绑定表达式，只用于简单操作。放入太多逻辑会让模板过重难以维护。
<div id="example">
<p>Original message: "{{ message }}"</p>
<p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
var vm = new Vue({
el: '#example',
data: {
message: 'Hello'
},
methods: {
    reverseMessage: function () {
        return this.message.split('').reverse().join('')
    }
}
computed: {
// a computed getter
reversedMessage: function () {
// `this` points to the vm instance
return this.message.split('').reverse().join('')
    }
}
})
这里我们声明了一个计算属性 reversedMessage 。我们提供的函数将用作属性 vm.reversedMessage 的 getter 。
computed: 计算属性，基于它的依赖缓存，只有它的相关依赖发生改变时才会重新取值，也就是message没有改变，多次访问，不会执行函数。函数里面没有访问变量的话，不会更新。
只能直接设值，不能异步设值
method：每次取值都计算，

计算set方法：
computed: {
fullName: {
// getter
get: function () {
return this.firstName + ' ' + this.lastName
},
// setter
set: function (newValue) {
var names = newValue.split(' ')
this.firstName = names[0]
this.lastName = names[names.length - 1]
}
}
}

$watch

Vue.js 提供了一个方法 $watch ，它用于观察 Vue 实例上的数据变动。当一些数据需要根据其它数据变化时， $watch很诱人



###class与style
<div class="static"
v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

data: {
isActive: true,
hasError: false
}

<div class="static active"></div>

当 isActive 或者 hasError 变化时，class 列表将相应地更新


style：

<div v-bind:style="styleObject"></div>

data: {
styleObject: {
color: 'red',
fontSize: '13px'
}
}



<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>

<template v-if="ok">
<h1>Title</h1>
<p>Paragraph 1</p>
<p>Paragraph 2</p>
</template>

v-show

不同的是有 v-show 的元素会始终渲染并保持在 DOM 中。v-show 是简单的切换元素的 CSS 属性 display 。





vue  属于mvvm中的vm层

data是module
template和css属于view层



当一个Vue实例被创建时，vue的实例加入了data对象
当data对象中的属性发生改变，视图会重渲染。
只有实例被创建时把data中属性放在vm中的属性才是响应式的，所以新加的是没用的
（Vue 不能检测到对象属性的添加或删除）（Vue 不允许动态添加根级响应式属性）



vue 声明周期


beforeCreate (){
//this.$data = undefined 数据还没有初始化
//模板没有编译
//视图没有渲染
//this.$el = undefined 元素还没有挂载
}

created (){
//this.$data 数据已经初始化
//模板没有编译
//视图没有渲染
//this.$el = undefined 元素没有挂载
}

beforeMount (){
//this.$data 数据已经初始化
//模板已经编译
//视图没有渲染
//this.$el = undefined 元素没有挂载
}

mounted () {
//this.$data 数据已经初始化
//模板已经编译
//视图已经渲染
//this.$el 元素已经挂载
}

beforeUpdate () {
    //更新前调用
}

Update () {
    //更新后调用
}

beforeDestroy() {
    //销毁前调用
}

destroy () {
    //销毁后调用
}



mixins:[myMixins]

同名钩子函数都将被调用
mixin的函数先调用，组件自身的函数再调用

除了钩子函数其他命名冲突，调组件的方法



vue的响应式原理



数据变化后

setter  ==> watch ==> render (Vue 异步执行 DOM 更新。)



vue或者weex开发，在function内部可以创建一个的函数，而不是外部

function outer() {
    return inner()
}

function inner() {
    return 1+2
}

outer()

修改成：
function outer() {
    function inner() {
        return 1+2
    }
    return inner()
}
outer()


根据最小暴露原则，
inner()不能暴露，修改后无法被外部访问

对于Vue.set和vue响应的问题的总结
   不能检测到的数组变动是：
    1、当利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue；
    2、当修改数组的长度时，例如：vm.items.length = newLength；
   不能检测到的对象变动是：
    3、向响应式对象添加属性；
    4、向响应式对象删除属性；

如果出现上述情况下要使用Vue.set
比如需要在对象上添加新属性，也可以不用Vue.set
替代方案例如
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
详情可以看官网该方面介绍：https://cn.vuejs.org/v2/guide/reactivity.html



[官方文档](https://cn.vuejs.org/v2/guide/components-custom-events.html)

.sync 修饰符
2.3.0+ 新增

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。

这也是为什么我们推荐以 update:myPropName 的模式触发事件取而代之。举个例子，在一个包含 title prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图：
```js
this.$emit('update:title', newTitle)
```
然后父组件可以监听那个事件并根据需要更新一个本地的数据属性。例如：
```js
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```
为了方便起见，我们为这种模式提供一个缩写，即 .sync 修饰符：
```js
<text-document v-bind:title.sync="doc.title"></text-document>
```
注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 v-model。

当我们用一个对象同时设置多个 prop 的时候，也可以将这个 .sync 修饰符和 v-bind 配合使用：
```js
<text-document v-bind.sync="doc"></text-document>
```
这样会把 doc 对象中的每一个属性 (如 title) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。

将 v-bind.sync 用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，是无法正常工作的，因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。