## 事件循环
EventLoop是一个执行模型，在不同的有不同的实现，浏览器和NodeJS基于不同的技术实现了各自的EventLoop。
浏览器的EventLoop是在HTML5规范中明确定义了的
NodeJS的EventLoop是基于libuv实现的。可以在libuv官网和NodeJS官网查看
eventloop 是为了解决在单线程环境下执行多个任务

EventLoop是浏览器开启的线程，作用是持续的检测调用栈和回调队列，如果检测到**调用栈为空**，它就会通知微队列和宏队列，把队列中的第一个回调函数推入执行栈，交给 js 引擎执行
todo: 如何监听

```js
// eventLoop是一个用作队列的数组 //(先进，先出)
var eventLoop = [ ];
var event;
// “永远”执行 while (true) {
// 一次tick
if (eventLoop.length > 0) {
// 拿到队列中的下一个事件
event = eventLoop.shift();
// 现在，执行下一个事件
    try {
        event(); 
    } catch (err) {
        reportError(err);
        } 
    }
}
```

## 宏队列和微队列
宿主可以发起的任务，在ES5之后，JavaScript引入了Promise，JavaScript引擎本身也可以发起任务了
我们把宿主发起的任务称为宏观任务，把JavaScript引擎发起的任务称为微观任务。

## 宏任务
macrotask，也叫 tasks，主要的工作如下
创建主文档对象,解析HTML,执行主线或者全局的javascript的代码,更改url以及各种事件。
页面加载,输入，网络事件，定时器。从浏览器角度看，宏任务是一个个离散的，独立的工作单元。
运行完成后，浏览器可以继续其他调度，重新渲染页面的UI或者去执行垃圾回收
* setTimeout
* setInterval
* setImmediate (Node)
* requestAnimationFrame (浏览器)
* I/O
* UI rendering (浏览器)
有些地方会列出来UI Rendering，说这个也是宏任务，可是在读了HTML规范文档以后，发现这很显然是和微任务平行的一个操作步骤
requestAnimationFrame姑且也算是宏任务吧，requestAnimationFrame在MDN的定义为，下次页面重绘前所执行的操作，而重绘也是作为宏任务的一个步骤来存在的，且该步骤晚于微任务的执行

## 微任务
microtask，也叫 jobs
* process.nextTick (Node)优先于其他的微任务执行。
* Promise.then()
* catch
* finally
* Object.observe
* MutationObserver （浏览器）
这里有一点需要注意的：Promise.then() 与 new Promise(() => {}).then() 是不同的，前面的是一个微任务，后面的 new Promise() 这一部分是一个构造函数，这是一个同步任务，后面的 .then() 才是一个微任务，这一点是非常重要的。
### Promise
async函数在await之前的代码都是同步执行的，可以理解为await之前的代码属于new Promise时传入的代码，await之后的所有代码都是在Promise.then中的回调
Promise是JavaScript语言提供的一种标准化的异步管理方式，它的总体思想是，需要进行io、等待或者其它异步操作的函数，不返回真实结果，而返回一个“承诺”，函数的调用方可以在合适的时机，选择等待这个承诺兑现（通过Promise的then方法的回调）。

Promise永远在微任务队列队列尾部添加微观任务。setTimeout等宿主API，则会添加宏观任务。

## 例子
### 初始执行的例子
刚开始我以为是1 2 4 3
就是先把宏任务放到执行栈执行的时候，
```js
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
})
console.log(2)
// 1,2,3, undefined，4
// 执行宏任务，
```
这段代码作为宏任务，进入主线程。
先遇到setTimeout，那么将其回调函数注册后分发到宏任务Event Queue。(注册过程与上同，下文不再描述)
接下来遇到了Promise，new Promise立即执行，then函数分发到微任务Event Queue。
遇到console.log()，立即执行。
好啦，整体代码script作为第一个宏任务(可以为一个函数)执行结束，看看有哪些微任务？我们发现了then在微任务Event Queue里面，执行。
ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中setTimeout对应的回调函数，立即执行。
结束。
为什么会有 undefined？
控制台将上面的一整段代码放在eval（）函数里面执行，执行完成以后返回了 undefined。
微任务会在下一轮任务开始前执行。


### 在微任务中创建微任务
执行顺序：主线程 => 主线程上创建的微任务1 => 微任务1上创建的微任务2 => 主线程上创建的宏任务
如果一直在微任务中创建微任务，可以是无限循环
```js
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
  Promise.resolve().then(_ => {
    console.log('before timeout')
  }).then(_ => {
    Promise.resolve().then(_ => {
      console.log('also before timeout')
    })
  })
})

console.log(2)
1
2
3
before timeout
also before timeout
4
```
下一个宏任务开始执行前会把微任务都先执行
### 宏任务中创建微任务
执行顺序：主线程 => 主线程上的宏任务队列1 => 宏任务队列1中创建的微任务
```js
// 宏任务队列 1
setTimeout(() => {
  // 宏任务队列 2.1
  console.log('timer_1');
  setTimeout(() => {
    // 宏任务队列 3
    console.log('timer_3')
  }, 0)
  new Promise(resolve => {
    resolve()
    console.log('new promise')
  }).then(() => {
    // 微任务队列 1
    console.log('promise then')
  })
}, 0)

setTimeout(() => {
  // 宏任务队列 2.2
  console.log('timer_2')
}, 0)

console.log('========== Sync queue ==========')

// 执行顺序：主线程（宏任务队列 1）=> 宏任务队列 2 => 微任务队列 1 => 宏任务队列 3
结果：

========== Sync queue ==========
timer_1
new promise
promise then
timer_2
timer_3
```
### 微任务队列中创建的宏任务
执行顺序：主线程 => 主线程上创建的微任务 => 主线程上创建的宏任务 => 微任务中创建的宏任务

异步宏任务队列只有一个，当在微任务中创建一个宏任务之后，他会被追加到异步宏任务队列上（跟主线程创建的异步宏任务队列是同一个队列）
```js
// 宏任务1
new Promise((resolve) => {
  console.log('new Promise(macro task 1)');
  resolve();
}).then(() => {
  // 微任务1
  console.log('micro task 1');
  setTimeout(() => {
    // 宏任务3
    console.log('macro task 3');
  }, 0)
})

setTimeout(() => {
  // 宏任务2
  console.log('macro task 2');
}, 1000)

console.log('========== Sync queue(macro task 1) ==========');
结果：
new Promise(macro task 1)
========== Sync queue(macro task 1) ==========
micro task 1
macro task 3
macro task 2

记住，如果把
setTimeout(() => { // 宏任务2 console.log('macro task 2'); }, 1000)改为立即执行setTimeout(() => { // 宏任务2 console.log('macro task 2'); }, 0)
那么它会在macro task 3之前执行，因为定时器是过多少毫秒之后才会加到事件队列里
```
## node老版本和浏览器的差异
```js
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
// 1 7 6 8 2 4 3 5 9 11 10 12
// node的环境结果是
1 7 6 8 2 4 9 11 3 10 5 12
// node v11前一个宏任务执行完，执行另外一个宏任务，再执行微任务
// node v11后就和浏览器一样了,每次一个宏任务后都会执行微任务
```

## 总结
微任务队列优先于宏任务队列执行，微任务队列上创建的宏任务会被后添加到当前宏任务队列的尾端，微任务队列中创建的微任务会被添加到微任务队列的尾端。只要微任务队列中还有任务，宏任务队列就只会等待微任务队列执行完毕后再执行。

