

todo: 继续整理
### XSS

> xss表示Cross Site Scripting(跨站脚本攻击)，XSS的攻击方式就是想办法“教唆”用户的浏览器去执行一些这个网页中原本不存在的前端代码。

### XSS的类型
总体来说，XSS分三类，存储型XSS、反射型XSS、DOM-XSS。
- 反射型
将用户输入的存在XSS攻击的数据，发送给后台，后台并未对数据进行存储，也未经过任何过滤，直接返回给客户端。被浏览器渲染。就可能导致XSS攻击；
   代码在project/imooc/xss
   流程是：发出请求时，xss代码出现在url中，查询参数，作为输入提交到服务器端，服务器端解析后响应，xss代码随响应内容一起传回给浏览器，浏览器解析执行xss代码
  >
  > 浏览器会发现xss攻击，需要res.set('X-XSS-Protection',0);
  > 
  >
  > xss代码存储在url中

  1. 植入攻击代码

     ```js
     localhost:3000?xss=<img src="null" onerror="alert(1)"/>
         //自动触发onerror
     ```

  2. 诱导

     ```js
     localhost:3000?xss=<p onclick="">点我</p>
     
     ```

  3. 植入广告

     ```js
     localhost:3000?xss=<iframe src="//baidu.com/t.html"/>
     ```

- 存储型

  > 和反射型的区别是代码会存储在服务器端（数据库，内存，文件系统等），下次请求目标页面时不用再提交xss代码

### 防御xss攻击

1. 编码

   html entity 编码

2. 过滤

   移除用户上传的dom属性，onerror

   移除用户上次的style，script，iframe标签

3. 校正

   避免直接对html entity解码

   使用dom parse 转换，校正不匹配的dom标签
   
   
实战：

vue提供v-html标签用来内嵌 html 代码
容易产生XSS，这个内容不能是用户提供的插值
如何防止：

jsxss 库import xss from 'xss'
原理： 根据白名单过滤HTML


### 参考
[XSS 攻击和防御详解](https://juejin.im/entry/58a598dc570c35006b5cd6b4)
