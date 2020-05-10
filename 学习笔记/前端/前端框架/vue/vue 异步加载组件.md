const Foo = resolve => require(['./Foo.vue'], resolve)
//或者
const Foo = () => import('./Foo’);

// 生产环境切换到懒加载
const _import = require('./_import_' + process.env.NODE_ENV);
const Foo = _import('Foo');



Vue.js 允许将组件定义为一个工厂函数，动态地解析组件的定义。Vue.js 只在组件需要渲染时触发工厂函数，

Vue.component('async-example', function (resolve, reject) {
setTimeout(function () {
// Pass the component definition to the resolve callback
resolve({
template: '<div>I am async!</div>'
})
}, 1000)
})

new Vue({
// ...
components: {
'my-component': () => import('./my-async-component')
}
})
vue在需要渲染时候触发工厂函数，就是 () => import('./my-async-component’)
然后把结果缓存起来用于后面再次渲染 

Vue.component('async-webpack-example', function (resolve) {
// 这个特殊的 require 语法告诉 webpack
// 自动将编译后的代码分割成不同的块，
// 这些块将通过 Ajax 请求自动下载。
require(['./my-async-component'], resolve)
})





参考资料：
https://router.vuejs.org   vue-router   懒加载

https://cn.vuejs.org/v2/guide/components.html#%E5%BC%82%E6%AD%A5%E7%BB%84%E4%BB%B6   vue  异步组件

https://webpack.js.org/guides/code-splitting/  webpack 代码分割