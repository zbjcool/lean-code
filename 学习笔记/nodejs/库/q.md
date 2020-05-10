以下几种方法可以把nodejs中包含callback回调的函数转化为promise风格的代码：
* Q.nfcall
* Q.nfapply
* Q.defer
* Q.denodeify

为了让前端们从回调的地狱中回到天堂，jQuery也引入了Promise的概念。Promise是一种令代码异步行为更加优雅的抽象，有了它，我们就可以像写同步代码一样去写异步代码。
promise 是抽象异步处理对象以及对其进行各种操作的组件


使用了回调函数的异步处理
 getAsync("fileA.txt", function(error, result){ 
if(error){// 取得失败时的处理 throw error; 
} // 取得成功时的处理}); 
---- <1> 传给回调函数的参数为(error对象， 执行结果)组合

下面是使用了Promise进行异步处理的一个例子—— 
var promise = getAsyncPromise("fileA.txt"); promise.then(function(result){ // 获取文件内容成功时的处理}).catch(function(error){ // 获取文件内容失败时的处理}); ---- <1> 返回promise对象

这和回调函数方式相比有哪些不同之处呢？ 在使用promise进行一步处理的时候，我们必须按照接口规定的方法编写处理代码。
除promise对象规定的方法(这里的 then 或 catch)以外的方法都是不可以使用的， 而不会像回调函数方式那样可以自己自由的定义回调函数的参数，而必须严格遵守固定、统一的编程方式来编写代码。

var promise = new Promise(function(resolve, reject) { // 异步处理 // 处理结束后、调用resolve 或 reject
if (isSuccess()){
    resolve(data);
} else {
    reject(error
)
}
});


function asyncFunction() { 
return new Promise(function (resolve, reject) {                     setTimeout(function () { 
resolve('Async Hello world'); }, 16); }); 
} asyncFunction()
.then(function (value) 
{ console.log(value); // => 'Async Hello world’})
.catch(function (error) { 
console.log(error); 
});

如果不像用catch
asyncFunction().then(
function (value) { 
console.log(value); 
}, 
function (error) { 
console.log(error);
});


Promise.resolve 和 Promise.reject这两个方法
Promise.resolve(42);  相当于
new Promise(function(resolve){ resolve(42); });

Promise.resolve(42).then(
function(value){ 
console.log(value); 
});
