
## 词法
### 为什么12.toString会报错？
```JavaScript
12.toString()
```
<details><summary><b>答案</b></summary>
<p>
因为小数点前面和后面的数字可以省略，所以这时候12.toString()会被看成一个整体，所以我们要想让点单独成为一个token，就要加入空格，这样写：
12 .toString()
</p>
</details>

### 用零宽空格和零宽连接符、零宽非连接符，写一段好玩的代码。 x


### 输出是什么？

```javascript
function sayHi() {
  console.log(name)
  console.log(age)
  var name = 'Lydia'
  let age = 21
}

sayHi()
```

- A: `Lydia` 和 `undefined`
- B: `Lydia` 和 `ReferenceError`
- C: `ReferenceError` 和 `21`
- D: `undefined` 和 `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

答案: D

在函数内部，我们首先通过 `var` 关键字声明了 `name` 变量。这意味着变量被提升了（内存空间在创建阶段就被设置好了），直到程序运行到定义变量位置之前默认值都是 `undefined`。因为当我们打印 `name` 变量时还没有执行到定义变量的位置，因此变量的值保持为 `undefined`。

通过 `let` 和 `const` 关键字声明的变量也会提升，但是和 `var` 不同，它们不会被<i>初始化</i>。在我们声明（初始化）之前是不能访问它们的。这个行为被称之为暂时性死区。当我们试图在声明之前访问它们时，JavaScript 将会抛出一个 `ReferenceError` 错误。

</p>
</details>


### 一元操作符

```javascript
+true;
!"Lydia";
```

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`

<details><summary><b>答案</b></summary>
<p>

答案: A

一元操作符加号尝试将 bool 转为 number。`true` 转换为 number 的话为 `1`，`false` 为 `0`。

字符串 `'Lydia'` 是一个真值，真值取反那么就返回 `false`。

</p>
</details>

### 数据类型的隐式转换
```js
let a = 3
let b = new Number(3)
let c = 3

console.log(a == b)
console.log(a === b)
console.log(b === c)
```
A: true false true
B: false false true
C: true false false
D: false true true
<details><summary><b>答案</b></summary>
<p>

答案：c
== 会先调用[[ToPrimitive]]方法，又会根据情况不同，判断先执行对象的 valueOf 方法还是 toString 方法进行准换，比如 + 和 == 运算符，当 + 运算符的时候，更倾向于转成字符串，而当 == 的时候，更倾向于转为数字
</p>
</details>

## 作用域

### 变量提升
```js
var a = 99;
f();
console.log(a);
function f() {
  console.log(a);
  var a = 10;
  console.log(a);
}
```
<details><summary><b>答案</b></summary>
<p>
undefined 10 99
</p>
</details>

```js
if (!('a' in window)) {
  var a = 'hello'
}
console.log(a)
```

<details><summary><b>答案</b></summary>
<p>

输出：undefined
相当于
```js
var a = undefined;
if (!('a' in window)) {
  a = 'hello'
}
console.log(a)
```
</p>
</details>

### 预处理

```js
var a = 1;

function foo() {
    console.log(a);
    var a = 2;
}

foo();
```
```js
var a = 1;

function foo() {
    console.log(a);
    if(false) {
        var a = 2;
    }
}

foo();
```

<details><summary><b>答案</b></summary>
<p>
答案都是 undefined，变量 a 提升了，打印 a 的时候查找作用域，全局的 a 被覆盖了，所以是 undefined，var 定义的变量不会在块作用力里面所以第二个和第一个一样
</p>
</details>

### 遍历
```js

for(var i = 0; i < 20; i ++) {
    void function(i){
        var div = document.createElement("div");
        div.innerHTML = i;
        div.onclick = function(){
            console.log(i);
        }
        document.body.appendChild(div);
    }(i);
}

```
<details><summary><b>答案</b></summary>
<p>
每个按钮里面的 i 是0到20
</p>
</details>

### let const
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1)
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1)
}
```

- A: `0 1 2` 和 `0 1 2`
- B: `0 1 2` 和 `3 3 3`
- C: `3 3 3` 和 `0 1 2`

<details><summary><b>答案</b></summary>
<p>

答案: C

由于 JavaScript 的事件循环，`setTimeout` 回调会在*遍历结束后*才执行。因为在第一个遍历中遍历 `i` 是通过 `var` 关键字声明的，所以这个值是全局作用域下的。在遍历过程中，我们通过一元操作符 `++` 来每次递增 `i` 的值。当 `setTimeout` 回调执行的时候，`i` 的值等于 3。

在第二个遍历中，遍历 `i` 是通过 `let` 关键字声明的：通过 `let` 和 `const` 关键字声明的变量是拥有块级作用域（指的是任何在 {} 中的内容）。在每次的遍历过程中，`i` 都有一个新值，并且每个值都在循环内的作用域中。

</p>
</details>

## 语法
### let const var 区别

<details><summary><b>答案</b></summary>
<p>
var 只有函数作用域，全局作用域，变量提升，可以重复声明
let 支持块作用域，暂时性死区，不能重复声明
const 是常量
</p>
</details>

## 数据类型



### js 有哪些数据类型 如何判断? 
<details><summary><b>答案</b></summary>
<p>
null, undefined, string, number, boolean, bigint, symbol, object
用typeof判断，除了 null 有点特殊是 object
</p>
</details>

<details><summary><b>错误</b></summary>
<p>
新增 bigint和 symbol
</p>
</details>

### null 和 undefined区别 应用场景?

<details><summary><b>答案</b></summary>
<p>
null 是字面量，null表示的是：“定义了但是为空”。指示变量未指向任何对象。把 null 作为尚未创建的对象，地址前三为是0，所以typeof判断为 object，

undefined 不能修改，但是函数内可以，所以判断会有错误，在实际编程时，我们一般不会把变量赋值为 undefined，这样可以保证所有值为 undefined 是变量，都是从未赋值的自然状态。
```js
function fn(){
  var undefined = 100;
  var u;
  console.log(undefined);  //chrome: 100,  ie8: 100
  console.log(u === undefined) // false 因为 undefined 被篡改了
  console.log(typeof u === 'undefined') // true 
  console.log(u === void 0) // true
}
fn();
```
</p>
</details>


### new String('a') 和 'a' 是一样的么?

<details><summary><b>答案</b></summary>
<p>

```js
var a = "foo";
var b = new String("foo");

console.log(a); // foo
console.log(b); // String {0: "f", 1: "o", 2: "o", length: 3, [[PrimitiveValue]]: "foo"}

console.log(typeof a); // "string"
console.log(typeof b); // "object"

console.log(a == b);  // true
console.log(a === b); // false

console.log(a.charAt === b.charAt); // true
```
a 是原始值，b是原始值类型 String 构造函数的实例，
当一个Object与一个String/Number类型比较时，会首先对Object调用[[toPrimitive]]方法，

因为当我们尝试访问一个primitive值的属性时，JS引擎内部会调用一个内置[[toObject]] 方法，转化为String 构造函数的实例

</p>
</details>

## js 数组
### js splice
<details><summary><b>答案</b></summary>
<p>

支持增，删，改（替换）
改或者叫替换其实是删除又增加

```js
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
直接修改原数组，返回删除元素的数组
// 增加
// deleteCount是0或者负数，后面加上要添加的 item, 不添加没有修改
var arr = [1, 2, 3];
arr.splice(0, 0) // 返回[]，arr: [1,2,3]
arr.splice(0, -1, 4); // 返回[], arr: [4, 1,2,3]
// 删除
// deleteCount 是正数，省略全删
arr.splice(0, 2) // 返回[1,2] arr:[3]
arr.splice(1) // 返回[2,3] arr:[1]
arr.splice(0) // 返回[1,2,3] arr:[]

// 改是在删除的情况下添加要增加的 item
arr.splice(0,2,5,6); // 返回[1,2] arr: [5,6,3]
```
</p>
</details>

### instanceof实现原理
<details><summary><b>答案</b></summary>
<p>

```javascript
function _instanceof (leftValue, rightValue) {
  let leftValue = Object.getPrototypeOf(leftValue);
  let rightValue = rightValue.prototype;
  while(true) {
    if (leftValue === null) return false;
    if (leftValue === rightValue) return true;
    leftValue = Object.getPrototypeOf(leftValue);
  }
}
```
</p>
</details>

### 如何判断是数组，typeof 可以吗
<details><summary><b>答案</b></summary>
<p>

typeof不可以
1. typeof
```js
typeof [] // object
```
所以无法判断
2. instanceof
```js
[] instanceof Array; // true
```
3. constructor
```js
var arr = [1,2]
arr.constructor === Array // true
```
但是如果修改了constructor，就不行了
```js
var a = []
a.contrtuctor = Object;
a.contrtuctor === Array // false
```
4. Object.prototype.toString.call()
```js
Object.prototype.toString.call([]) === '[object Array]' //true 
```
但是如果重写了toString方法
```js
Object.prototype.toString = () => {
  console.log('完蛋')
}
//调用String方法
const a = [];
Object.prototype.toString.call(a);
```
这样就不行了
5. Array.isArray()
```js
Object.prototype.toString = () => {
  console.log('完蛋')
}
//调用String方法
const a = [];
Array.isArray(a) // true
```
</p>
</details>

### 有哪些查找方法

<details><summary><b>答案</b></summary>
<p>

1. indexOf lastIndexOf
arr.indexOf(searchElement[, fromIndex])
找到返回索引，没有-1
只能找确定的值
采用===判断
```js
var arr = [1,2,3]
arr.indexOf(2); // 1
arr.indexOf(4); // -1
arr.lastIndexOf(3) // 2
```

2. includes
arr.includes(valueToFind[, fromIndex])
找到 true，没有找到 false
includes采用等值相等，所以可以找到 NaN
3. filter
和 find 的区别是，会查找所有，返回数组
```js
arr.filter(function (ele) {
  console.log(this);
  return ele > 1
}, {a: 1})
// {a: 1} {a: 1} {a: 1} [2,3]
```
4. find和 findIndex
不同于 indexOf只能找指定的值，可以传函数
```js
var arr = [1,2,3]
arr.find((element,index,array) => {
  console.log(this);
  if (element > 1) {
    return true;
  }
}) // window window 2
arr.find((element,index,array) => element > 1) // 2
arr.find(function (ele) {
  console.log(this);
  return ele > 1
}, {a: 1})
// {a: 1} {a: 1} 2
```
findIndex和 find 的区别就是返回的是索引
```js
arr.findIndex(function (ele) {
  console.log(this);
  return ele > 1
}, {a: 1})
// {a: 1} {a: 1} 1
```

总结
indexOf 和 includes 查找指定，区别是 includes 不会返回索引
find和 filter 会根据条件查找，区别是filter 会查找所有元素

</p>
</details>


### arraylike 问题
<details><summary><b>答案</b></summary>
<p>

1. 和数组一样可读写，可以遍历，可以取length
```js
function foo() {
  let arr = Array.from(arguments)
  arguments[0];
  arguments.length;
  console.log(...arguments)
  console.log(arr)
}
foo(1,2,3)
```

2. arraylike转换成 array
```js
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 0. map
Array.prototype.map.call(arrayLike,(item) => item); 
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"] 
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"] 
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"] 
// 4. apply
Array.prototype.concat.apply([], arrayLike)

```
</p>
</details>

### 数组遍历有哪些，优缺点？都用在什么地方？
<details><summary><b>答案</b></summary>
<p>

1. every,some 
不支持 break，continue
遍历判断
对所有元素进行判断为 true，才返回 true
some() 有一个 true，就是 true
```js
[1,2,3].every((ele) => ele > 2) // false
[1,2,3].every((ele) => ele > 0) // true
[1,2,3].some((ele) => ele > 2) // true
```
2. reduce
不支持 break，continue
遍历累加
```js
[1,2,3].reduce((acc,cur) => acc + cur, 3) // 9
[1,2,3].reduce((acc,cur) => acc + cur) // 6
```

3. map 
不支持 break,continue
遍历修改每个元素
```js
[1,2,3].map((ele) => ele ** 2) // [1,4,9]
```


4. forEach 
不支持 break,continue
通用遍历
arr.forEach(callback(currentValue [, index [, array]])[, thisArg])

5. for 太笨

6. for/in不推荐，回忆下为什么不推荐


</p>
</details>

### 一行代码实现数组去重？js数组的掌握，不能使用ES6语法。
<details><summary><b>答案</b></summary>
<p>

1. 数组元素比较型
取出一个元素，将其与其后所有元素比较，若在其后发现相等元素，则将后者删掉
双重 for 循环
```js
var arr = [1,2,2,4,4,5]
for (let i = 0, len = arr.length; i < len; i++) {
  for (let j = i + 1, len = arr.length; j <len; j++) {
    if (arr[i] === arr[j]) {
      arr.splice(j, 1);
      j--;
      len--;
    }
  }
}
```
由于 NaN === NaN 等于 false，所以重复的 NaN 并没有被去掉

排序并进行相邻比较
先对数组内元素进行排序，再对数组内相邻的元素两两之间进行比较，经测试，该方法受限于 sort 的排序能力，所以若数组不存在比较复杂的对象(复杂对象难以排序)，则可尝试此方法
```js
function uniq(arr) {
    arr.sort();
    for (let i = 0; i < arr.length - 1; i++) {
        arr[i] === arr[i + 1] && arr.splice(i + 1, 1) && i--;
    }
    return arr;
}
// 运行结果
//[[], [], 1, "1", [1], NaN, NaN, {}, {}, { a: 1 }, {}, { a: 1 }, "a", null, undefined];
// 与 lodash 结果相比 NaN 没有去掉，且对象的去重出现问题
```
复制代码同样由于 NaN === NaN 等于 false，所以重复的 NaN 并没有被去掉，并且由于 sort 没有将对象很好的排序，在对象部分，会出现一些去重失效



2. 查找数组元素位置型
该类型即针对每个数组元素进行一次查找其在数组内的第一次出现的位置，若第一次出现的位置等于该元素此时的索引，即收集此元素 

filter和 indexOf或者 findIndex
```js
[1,2,2,4,4,5].filter((ele,index,arr) => arr.indexOf(ele) === index)

[1,2,2,4,4,5].filter((ele,index,arr) => {
  return arr.findIndex(item => item === ele) === index
})

```
由于 indexOf 用===判断，而 NaN 和其他所有元素都不相等，所以 NaN 会没有

includes
```js
function uniq(arr) {
    let res = [];
    for (let i = 0; i < arr.length; i++) {
        if (!res.includes(arr[i])) {
            res.push(arr[i]);
        }
    }
    return res;
}
```
includes 方法采用 SameValueZero 判断规则，所以可以判断出并去重 NaN
3. 依托数据类型特性型
* map
```js
function uniq(arr) {
    let map = new Map();
    for (let i = 0; i < arr.length; i++) {
        !map.has(arr[i]) && map.set(arr[i], true);
    }
    return [...map.keys()];
}

```

* set也是等值相等
```js
function distinct(array) {
   return Array.from(new Set(array));
}
console.log(unique([NaN, NaN])) // [NaN]

function uniq(arr) {
    return [...new Set(arr)];
}
```

* Object 键值对
```js
function distinct(array) {
    var obj = {};
    return array.filter(function(item, index, array){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
}
```
object 只能存字符串，会把对象转成字符串
number1: 1
numberNaN: NaN
object1: String {"1"}
object/a/: /a/
objectnull: null
string1: "1"
undefinedundefined: undefined



4. lodash 如何实现去重
简单说下 lodash 的 uniq 方法的源码实现。
这个方法的行为和使用 Set 进行去重的结果一致。
当数组长度大于等于 200 时，会创建 Set并将 Set 转换为数组来进行去重（Set 不存在情况的实现不做分析）。当数组长度小于 200 时，会使用类似前面提到的 双重循环 的去重方案，另外还会做 NaN 的去重。

总结性能，
一般情况下 Object 键值对 最快，其次set（对象不去重）, indexOf(NaN 会被忽略），
Object 键值对增加了空间复杂度

几种去重函数针对带有特殊类型的对比
双层 for, sort： 对象和 NaN 不去重
filter+indexOf: 对象不去重 NaN 会被忽略掉,indexOf底层使用了===判断，所以不能 NaN 去重
set, includes: 对象不去重 NaN 去重
Object 键值对: 都可以去重，因为 object 的 key 只能是字符串

</p>
</details>
## 异步

### js promise 和 settimeout 是宏任务还是微任务？

<details><summary><b>答案</b></summary>
<p>
promise是微任务，settimeout 是宏任务

```js
for (var i = 0; i <10; i++) {  
  setTimeout(function() {  // 同步注册回调函数到 异步的 宏任务队列。
    console.log(i);        // 执行此代码时，同步代码for循环已经执行完成
  }, 0);
}
// 输出结果
10   共10个
// 这里面的知识点： JS的事件循环机制，setTimeout的机制等
```
</p>
</details>

### Promise的用法以及实现原理。


## 函数

### 不定参数问题

### js ...运算符

## 对象

### js Set 和 Map 数据结构,set map Array 区别

set 类似 array ，没有重复值

set的 key 和 value 是一样

set和 map 一样 key 可以添加各种类型
如果是引用类型，内存地址不一样就当成两个

### var obj = Object.create(null)和 var obj = {}的区别

<details><summary><b>答案</b></summary>
<p>

```js
var obj = {};
Reflect.getPrototypeOf(obj);

var obj = Object.create(null)
Reflect.getPrototypeOf(obj);// null
```
Object.create(null)是真的空对象，没有原型

</p>
</details>

### js原型链：能说出原型链的始末
<details><summary><b>答案</b></summary>
<p>
概念
原型链是一种机制，指的是JavaScript每个对象包括原型对象都有一个内置的[[prototype]]属性指向创建它的函数对象的原型对象，即prototype属性。
作用
原型链的存在，主要是为了实现对象的继承。

![1901343902-57482ad01df6f_articlex.png](https://i.loli.net/2020/05/09/ZU7gC2VdB9Ms5LN.png)

</p>
</details>

### 一个关于 this 指向的问题

<details><summary><b>答案</b></summary>
<p>

this在 JAVA 中 指向当前类的实例

但是 js 没有类，为了模拟出 java 中的 this


隐式绑定，显式绑定

构造函数中的this，普通函数的 this，箭头函数中的 this
异步函数，生成器函数都是普通函数

this指向函数调用的上下文
相同的函数，使用不用的上下文
比如
```js
function showThis(){
    console.log(this);
}
var o = {
    showThis: showThis
}
showThis(); // global
o.showThis(); // o
```
执行o.showThis 的时候返回 reference，reference 类型有两个值，分别是对象和属性值，调用函数的时候，

普通函数
1. this 在全局中
指向全局对象
2. this在函数中
动态作用域，this 执行调用者
3. 构造函数
this指向构造函数实例
箭头函数
this 指向上一级的的 this
</p>
</details>

### 怎么判断两个对象相等？
<details><summary><b>答案</b></summary>
<p>

1. 如果是浅拷贝

```js
var obj1 = {
  name: 'foo'
}
var obj2 = obj1;
obj1 === obj2; // true
```
2. 深拷贝
```js
/*
 * @param x {Object} 对象1
 * @param y {Object} 对象2
 * @return  {Boolean} true 为相等，false 为不等
 */
var deepEqual = function (x, y) {
  // 指向同一内存时
  if (x === y) {
    return true;
  }
  else if ((typeof x == "object" && x != null) && (typeof y == "object" && y != null)) {
    if (Object.keys(x).length != Object.keys(y).length)
      return false;

    for (var prop in x) {
      if (y.hasOwnProperty(prop))
      {  
        if (! deepEqual(x[prop], y[prop]))
          return false;
      }
      else
        return false;
    }

    return true;
  }
  else 
    return false;
}
```
最后附上Underscore里的_.isEqual()源码地址： [github.com/hanzichi/un…](https://github.com/lessfish/underscore-analysis/blob/master/underscore-1.8.3.js/src/underscore-1.8.3.js#L1094-L1190)
</p>
</details>


### 普通函数和箭头函数 this 的区别
```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2
  },
  perimeter: () => 2 * Math.PI * this.radius
}

shape.diameter()
shape.perimeter()
```

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

<details><summary><b>答案</b></summary>
<p>

答案: B

注意 `diameter` 的值是一个常规函数，但是 `perimeter` 的值是一个箭头函数。

对于箭头函数，`this` 关键字指向的是它当前周围作用域（简单来说是包含箭头函数的常规函数，如果没有常规函数的话就是全局对象），这个行为和常规函数不同。这意味着当我们调用 `perimeter` 时，`this` 不是指向 `shape` 对象，而是它的周围作用域（在例子中是 `window`）。

在 `window` 中没有 `radius` 这个属性，因此返回 `undefined`。

</p>
</details>


### set和object 区别
```javascript
const obj = { 1: 'a', 2: 'b', 3: 'c' }
const set = new Set([1, 2, 3, 4, 5])

obj.hasOwnProperty('1')
obj.hasOwnProperty(1)
set.has('1')
set.has(1)
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>答案</b></summary>
<p>

 答案: C

所有对象的键（不包括 Symbol）在底层都是字符串，即使你自己没有将其作为字符串输入。这就是为什么 `obj.hasOwnProperty('1')` 也返回 `true`。

对于集合，它不是这样工作的。在我们的集合中没有 `'1'`：`set.has('1')` 返回 `false`。它有数字类型为 `1`，`set.has(1)` 返回 `true`。

</p>
</details>


### object的 key 是 object
```javascript
const a = {}
const b = { key: 'b' }
const c = { key: 'c' }

a[b] = 123
a[c] = 456

console.log(a[b])
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>答案</b></summary>
<p>

答案: B

对象的键被自动转换为字符串。我们试图将一个对象 `b` 设置为对象 `a` 的键，且相应的值为 `123`。

然而，当字符串化一个对象时，它会变成 `"[object Object]"`。因此这里说的是，`a["[object Object]"] = 123`。然后，我们再一次做了同样的事情，`c` 是另外一个对象，这里也有隐式字符串化，于是，`a["[object Object]"] = 456`。

然后，我们打印 `a[b]`，也就是 `a["[object Object]"]`。之前刚设置为 `456`，因此返回的是 `456`。

</p>
</details>

---



## js 拷贝

### 说一下深拷贝的实现原理。

<details><summary><b>答案</b></summary>
<p>


</p>
</details>


## js 面向对象开发
* 面向对象如何实现? 需要复用的变量 怎么处理 ?
