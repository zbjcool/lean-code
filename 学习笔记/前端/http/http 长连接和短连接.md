## 为什么有长连接
http基于请求/响应模式，
因此只要服务端给了响应，本次 http 请求就结束了

## 长连接和短连接本质
http 长连接和短连接本质上是 TCP 长连接和短连接

http 就是寄快递的快递单
tcp 是送货的大货车走的路

http 是通过 tcp 传输的

## 默认情况
http 1.0中默认是关闭的，默认使用短连接。
需要在http头加入"Connection: Keep-Alive"，才能启用Keep-Alive；http 1.1中默认启用Keep-Alive，如果加入"Connection: close "，才关闭。
Connection: keep-alive 
TCP 连接不会关闭
如果客户端再次访问这个服务器，继续使用这条连接
Connection: close

## 短连接
建立连接-数据传输-关闭连接...建立连接-数据传输-关闭连接
客户端和服务端都可以发起关闭连接，不过一般服务端不会马上发起关闭连接，都是客户端发起

## 长连接
建立连接-数据传输...(保持连接)数据传输-关闭连接
省去建立连接时间
如果服务端一直不关闭，也不行
question: 长连接关闭策略




当HTTP采用keepalive模式，当客户端向服务器发生请求之后，客户端如何判断服务器的数据已经发生完成？
[HTTP Keep-Alive模式](https://www.cnblogs.com/skynet/archive/2010/12/11/1903347.html)
