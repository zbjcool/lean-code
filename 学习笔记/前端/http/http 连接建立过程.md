# http 连接的建立

在浏览器输入网址，点击确认，意味浏览器要向该网址发送一个网页请求的数据包。
## DNS

使用主机名（ip 地址）或者域名进行访问
如果是域名先进行 DNS 域名解析，
DNS 域名解析过程：
（域名解析成ip地址，走UTP协议，因此不会有握手过程）：浏览器将 URL 解析出相对应的服务器的 IP 地址
1. host 文件，本地DNS 解析器缓存
2. 查找首选的 DNS 服务器，8.8.8.8是谷歌的 SNS,114.114.114.114是国内 ISP 提供的 DNS
3. 没有转发模式，直接发给 根DNS递归搜索，转发模式，一层一层往上查询

## cdn
先根据 cdn 查看那个服务器离我最近，再根据 dns查找 ip 地址


### dig 工具
```bash
dig math.stackexchange.com
; <<>> DiG 9.10.6 <<>> www.sqmall.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43816
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;www.sqmall.com.			IN	A

;; ANSWER SECTION:
www.sqmall.com.		35	IN	A	118.31.247.68

;; Query time: 18 msec
;; SERVER: 114.114.114.114#53(114.114.114.114)
;; WHEN: Sat Apr 04 11:46:54 CST 2020
;; MSG SIZE  rcvd: 59
```
todo: 详细了解
[DNS 原理入门](http://www.ruanyifeng.com/blog/2016/06/dns.html)

## 发送
### 判断是否在同一个子网络
接下来，我们要判断，这个IP地址是不是在同一个子网络，这就要用到子网掩码。

已知子网掩码是255.255.255.0，本机用它对自己的IP地址192.168.1.100，做一个二进制的AND运算（两个数位都为1，结果为1，否则为0），计算结果为192.168.1.0；然后对Google的IP地址172.194.72.105也做一个AND运算，计算结果为172.194.72.0。这两个结果不相等，所以结论是，Google与本机不在同一个子网络。

因此，我们要向Google发送数据包，必须通过网关（路由）192.168.1.1转发，也就是说，接收方的MAC地址将是网关的MAC地址。
### 如果在同一个子网络
通过 ARP 协议，根据 IP 地址获取 mac 地址，用广播的方式发送包

### 不在同一个子网络
mac 地址为网关的 mac 地址，ip 是goods 的 ip，
网关收到后修改 mac 地址为下一个路由的 mac 地址

## TCP三次握手
todo: 详细了解三次握手过程

为什么三次握手：

### http
浏览器向服务器发送一条 HTTP 请求报文
服务器返回给浏览器一条 HTTP 响应报文
浏览器进行渲染
关闭 TCP 连接（四次挥手）