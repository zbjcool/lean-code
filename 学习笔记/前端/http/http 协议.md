# 历史
1990年 Tim Berners-lee 提出
成立了w3c，IETF
rfc（请求意见稿）是 IETF发布的备忘录，记录了协议，规范等标准文件
1991 http0.9
1996 http1.0
1997 http1.1 rfc1945
2015 http2.0

http3.0 其实就是 quic
QUIC和webRTC 基于 RUDP

# 特点
## 客户/服务器模式
## 灵活： content-type
## 无连接
每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接，*为了尽快释放资源*，可以是后面的网页越来越复杂，keep-alive，避免重新建立 TCP连接，只要没有断开，就可用这条TCP连接
## 无状态
无状态也是为了不要每次都带上上次的信息

后续处理如果需要前面的信息
cookie 和 session

# 参考
[HTTP 协议入门](https://www.ruanyifeng.com/blog/2016/08/http.html)
[rfc协议](https://www.w3.org/Protocols/rfc2616/rfc2616.html)