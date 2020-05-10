
### 正则表达式如何匹配一段url 

### ?在正则表达式中有哪几种作用?

### 验证正则，查看走向
[regulex](https://jex.im/regulex/#!flags=&re=%5E(a%7Cb)*%3F%24)
### 匹配图片结尾
\.(png|jpe?g|gif|svg)(\?.*)?$
### 给价格标逗号
一个很长的数字中每三位间加一个逗号(当然是从右边加起了)
((?<=\d)\d{3})+\b

### 查找这样的单词
* 出现字母q
\b\w*q\w*\b
* 它里面出现了字母q,但是q后面跟的不是字母u
\b\w*q[^u]\w*\b 查找Iraq fighting，返回Iraq fighting，不行
\b\w*q(?!u)\w*\b 可以


### 美国邮编的规则是5位数字，或者用连字号间隔的9位数字
错误：/\d{5}|\d{5}-\d{4}/ 匹配五位数字
正确：/\d{5}-\d{4}|\d{5}/


### ip 地址匹配表达式
(\d{1,3}\.){3}\d{1,3}


### 获取引号内的内容
```js
var reg = /['"][^'"]*['"]/
```
要求引号匹配的
```js
/(['"])[^'"]*\1/
```
不允许双引号内有单引号，反之亦然
```js
/(['"])[^\1]*\1/
```

### 驼峰格式字符串和下划线分割字符串转换
* 输入 驼峰格式字符串转换下划线分割字符串
比如getNameAndType。输出下划线分割字符串，比如get_name_and_type
```js
'getNameAndType'.replace(/([A-Z])/g, '_$1').toLowerCase()
```
```js
'getNameAndType'.replace(/[A-Z]/g, function (word) {
    return '_' + word.toLowerCase();
})
//或者 
str.replace(/([A-Z])/g, “_$&”).toLowerCase();
```

* var s1 = "get-element-by-id"; 给定这样一个连字符串，
写一个function转换为驼峰命名法形式的字符串 getElementById
```js
"get-element-by-id".replace(/-([a-z])/g, function(world,p1) {
    return p1.toUpperCase();
})
"get-element-by-id".replace(/-([a-z])/g, function(world) {
    return world.slice(1).toUpperCase();
})
```
### 判断字符串是否包含数字
```js
"sdfa11sdf".search(/\d/) > 0
/\d/.test("sdfa11sdf")
```

### 判断手机号码
```js
/^1[345678]\d{9}$/.test('17767165463')
```

### 密码强度正则，最少6位，包括至少1个大写字母，1个小写字母，1个数字
```js
/^.*(?=.{6,})(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).*$/
```

```js
/^.*(?=.{6,})(?=.*\d).*$/
// 这里是(?=.*\d)不是(?=\d)
/^.*(?=.{6,})(?=\d).*$/
// 这个正则的意思是，任意个字符位置的后面要有6个以上，而且是这个位子上有个数字
// regx1.exec('1zzzzz') 匹配上
// regx1.exec('z1zzzz') 匹配不上
// regx1.exec('z1zzzzz') 匹配上
```

### 过滤HTML标签
我写的
```js
var str="<p>dasdsa</p>nice <br> test</br>"
str.replace(/(<\/?.+?>)/gi, "");
```

答案
```js
var str="<p>dasdsa</p>nice <br> test</br>"
var regx = /<[^<>]+>/g;
str = str.replace(regx, '');
```

### url 解析

### userAgent 解析


### 
```js
str = `"aa" and "bb" and "cc"'`
提取aa，bb，cc
```


### 请把'$foo %foo foo'中前面是$符号的 foo 替换成 bar

### 请提取'$1 is worth about ￥123'字符中的美元数量四多少