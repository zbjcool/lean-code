## http常用状态码

### 1XX
Infomational（信息性状态码）接收的请求正在处理
### 2XX
Success（成功状态码）请求正常处理完毕
200 OK
202 ACCepted 已接受，但未处理完成
204 No Content // x
206 成功处理了部分 GET 请求，断点续传用到
### 3XX
Redirection（重定向状态码）需要进行附加操作以完成请求
301 Moved Permanently

302 Found
304 Not Modified
<meta http-equiv="cache-control" content="no-cache"/>
不意味response一定不能存储在客户端



### 4XX
Client Error（客户端错误状态码）服务器无法处理请求
400 Bad Request

401 Unauthorized

403 Forbidden

404 Not Found

### 5XX
Server Error（服务器错误状态码）服务器处理请求出错
500 Internal Server Error // 程序bug

502 Bad Gateway

504 Gateway Time-out //请求超时





如果用post请求浏览器不会缓存
