# url
## url和 uri 区别
url 是 uri 的一种，但不是所有的 uri 都是 url
提供 支持的协议的是 url
tel: 1778248394 是 uri

## url组成
protocol 协议，常用的协议是http
hostname 主机地址，可以是域名，也可以是IP地址
port 端口 http协议默认端口是：80端口，如果不写默认就是:80端口path 路径 网络资源在服务器中的指定路径
parameter 参数 如果要向服务器传入参数，在这部分输入query 查询字符串 如果需要从服务器那里查询内容，在这里编辑fragment 片段 网页中可能会分为不同的片段，如果想访问网页后直接到达指定位置，可以在这部分设置Http报头
# 报文分析
## 结构
分为三部分
1. 请求行（request line）/响应行（response line）
2. request head/response head
3. request body/response body
## 权重
q=权重值
范围： 0-1
不指定默认1.0
优先返回权重高的
## 请求报文行
GET /v1/get_related_entry?src=web HTTP/1.1
HOST: www.baidu.com
1. 请求方法 GET POST 等
2. 请求 uri，uri和请求报文头的 host 组成完整的 url
3. http协议以及版本
## 响应报文行
HTTP/1.1 200 Ok
1. 报文的协议以及版本
2. 状态码及状态描述
## 通用报文头
Cache-Control: no-cache 控制缓存的行为
* Connection

* Content-Length: 212

## 请求报文头

* host

* referer
告诉服务器我是从哪个页面链接过来
* Accept
浏览器端可以接受的媒体类型
Accept: text/html
406 Non Acceptable
Accept: */*
* Accept-Encoding
接受编码，压缩的方法
* Accept-Language
接受的语言
Accept-Language: zh-cn,zh;q=0.7,en-us,en;q=0.3
* user-agent
客户端使用的操作系统和浏览器的名称和版本
## 响应报文头

* Server
Server: nginx/1.10.1 //http服务器种类和版本
* Date
Date: Wed, 11 Apr 2018 08:03:18 GMT 
* Cache-control
* Expires
* Set-Cookie
JSESSIONID=7F6E7C92B96618BFA3F5C741ADD27701; Path=/fauna/; HttpOnly // 要求客户端设置cookie
* X-Requested-With
X-Requested-With is a non-standard HTTP header which is mainly used to identify Ajax requests.
Most JavaScript frameworks send this header with value of XMLHttpRequest
在服务器端判断request来自Ajax请求(异步)还是传统请求(同步)
## 实体报文头
* Allow
* Content-Encoding

* Content-type
In responses, a Content-Type header tells the client what the content type of the returned content actually is
syntax：
Content-Type: text/html; charset=UTF-8
Content-Type: multipart/form-data; boundary=something

mine-type:
application/json
application/octet-stream 二进制，文件下载
application/x-www-form-urlencoded 表单

## Accept与Content-Type的区别
1. Accept属于请求头， Content-Type属于实体头。
Http报头分为通用报头，请求报头，响应报头和实体报头。请求方的http报头结构：通用报头|请求报头|实体报头
响应方的http报头结构：通用报头|响应报头|实体报头
2. Accept代表发送端（客户端）希望接受的数据类型。
比如：Accept：text/xml;&nbsp;代表客户端希望接受的数据类型是xml类型Content-Type代表发送端（客户端|服务器）发送的实体数据的数据类型。比如：Content-Type：text/html;&nbsp;代表发送端发送的数据格式是html。二者合起来。Accept:text/xml;Content-Type:text/html;即代表希望接受的数据类型是xml格式，本次请求发送的数据的数据格式是html。
