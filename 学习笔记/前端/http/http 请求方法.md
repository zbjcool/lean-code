# 请求方法
* GET
目的是为了获取响应body
通过 URI 来识别资源。uri 作为 key ，浏览器缓存内容
不修改服务器
参数在请求url 用?开头，&拼接
明文请求，参数可以被所有人看到
get请求也可以在request body中放入数据
* POST
 目的不是获取响应 body
 也可以在请求url中加上参数，只要服务端接受参数
* PUT
是幂等的，post 是不幂等的
put 安全性问题，不怎么用
* HEAD
和 get 几乎相同，测试超链接有效性
* DELETE
* OPTIONS
查看支持的方法
* TRACE
测试和诊断，确认连接过程，会触发跨站追踪
* CONNECT
创建双向沟通的通道，用来创建隧道
## GET和 POST 的区别

get和post都是建立在tcp协议之上。
GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。
缓存和不缓存

那么回过头来想想为什么 HTTP 并未规定不可以 GET 中发送 Body 内容，但却不少知名的工具不能用 GET 发送 Body 数据，所以大致的讲我们仍然不推荐使用 GET 携带 Body 内容，还有可能某些应用服务器也会忽略掉 GET 的 Body 数据(???, 猜的)。我想更主要是 GET 被设计来用 URI 来识别资源，如果让它的请求体中携带数据，那么通常的缓存服务便失效了，URI 不能作为缓存的 Key。
[谁说 HTTP GET 就不能通过 Body 来发送数据呢？](https://yanbin.blog/why-http-get-cannot-sent-data-with-reuqest-body/#more-8193)