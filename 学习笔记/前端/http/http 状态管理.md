
# 状态管理
也被称为会话管理。
众所周知，HTTP 是一个无状态协议，所以客户端每次发出请求时，下一次请求无法得知上一次请求所包含的状态数据，如何能把一个用户的状态数据关联起来呢？
比如在淘宝的某个页面中，你进行了登陆操作。当你跳转到商品页时，服务端如何知道你是已经登陆的状态？
为了解决 http 无状态，产生了cookie和 seesion




客户端也有，服务端也有
cookie 存储在客户端，session 存储在服务端
cookie 类似会员卡，通行证
第一次来的客户，店长办一张会员卡，会员带上会员卡，店长刷卡，通过
## cookie

cookie 实际上是一小块文本信息
### 流程
1. 客户端请求服务器，浏览器查询这个域名下的 cookie，还要看有没有过期，如果有带上
2. 如果请求包含 cookie，服务器根据 cookie 的数据去查询数据库，如果找不到或者没有 cookie，而且服务器需要记录该用户状态， 服务器向客户端发送 set-cookie 头操作，并且在数据库保存数据
3. 浏览器根据是否有 set-cookie，将 cookie 保存。

注：cookie是服务器产生，客户端保存，每次请求都带上。
### set-cookie
无论使用何种服务端技术，只要发送回的HTTP响应中包含如下形式的头，则视为服务器要求设置一个cookie：
```html
Set-cookie:name=name;expires=date;path=path;domain=domain
```
支持cookie的浏览器都会对此作出反应，即创建cookie文件并保存（也可能是内存cookie），
### cookie 是否失效
用户以后在每次发出请求时，浏览器都要判断当前所有的cookie中有没有没失效（根据expires属性判断）并且匹配了path属性的cookie信息，如果有的话，会以下面的形式加入到请求头中发回服务端：
    Cookie: name="zj";Path="/linkage"

###  cookie参数
其他可选的 cookie 参数会影响将 cookie 发送给服务器端的过程，主要有以下几种：
* path：表示 cookie 影响到的路径，匹配该路径才发送这个 cookie。
* expires 和 maxAge：告诉浏览器这个 cookie 什么时候过期，expires 是 UTC 格式时间，maxAge 是 cookie 多久后过期的相对时间。当不设置这两个选项时，会产生 session cookie，session cookie 是 transient 的，当用户关闭浏览器时，就被清除。一般用来保存 session 的 session_id。
* secure：当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效。
* httpOnly：浏览器不允许脚本操作 document.cookie 去更改 cookie。一般情况下都应该设置这个为 true，这样可以避免被 xss 攻击拿到 cookie。
session

### cookie的弊端
cookie 虽然很方便，但是使用 cookie 有一个很大的弊端，cookie 中的所有数据在客户端就可以被修改，数据非常容易被伪造，那么一些重要的数据就不能存放在 cookie 中了，而且如果 cookie 中数据字段太多会影响传输效率。为了解决这些问题，就产生了 session，

## session
session 中的数据是保留在服务器端的。
session 的运作通过一个 session_id 来进行。session_id 通常是存放在客户端的 cookie 中，比如在 express 中，默认是 connect.sid 这个字段，当请求到来时，服务端检查 cookie 中保存的 session_id 并通过这个 session_id 与服务器端的 session data 关联起来，行数据的保存和修改。
question: session_id 如何生成保护安全
### 流程
### 首次访问
1. 当你浏览一个网页时，服务端随机产生sid和对应的 session，setCookie中有connect.sid字段
2. 浏览器保存该 cookie

### 再次访问 
1. 当你下次访问时，浏览器带上 cookie
2. 服务端根据 cookie 里面的sid找到 session
### 有效期
* session超时失效，定时清除 session
* httpSession.invalidate()
* 如果存在内存中，进程被停止而失效
## cookie 和 session 的区别
* 存在的位置：
cookie 存在于客户端，临时文件夹中；session 存在于服务器的内存中，一个 session 域对象为一个用户浏览器服务
* 安全性
cookie 是以明文的方式存放在客户端的，安全性低，可以通过一个加密算法进行加密后存放；session 存放于服务器的内存中，所以安全性好
* 生命周期(以 20 分钟为例)
cookie 的生命周期是累计的，从创建时，就开始计时，20 分钟后 cookie 生命周期结束；
session 的生命周期是间隔的，从创建时，开始计时如在 20 分钟，没有访问 session，那么 session 生命周期被销毁。但是，如果在 20 分钟内（如在第 19 分钟时）访问过 session，那么，将重新计算 session 的生命周期。关机会造成 session 生命周期的结束，但是对 cookie 没有影响
* 访问范围
cookie 为多个用户浏览器共享；session 为一个用户浏览器独享

## question: 如果 cookie 被禁
### URL 重写： 
* 直接附加在 url 后面: /xxx/xxx;Sessionid=ddddd
* 查询条件：/xxx/xxx?Sessionid=ddddd
* 隐藏表单
###