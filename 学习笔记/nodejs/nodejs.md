1）简单

2）高性能，避免了频繁的线程切换开销

3）占用资源小，因为是单线程，在大负荷情况下，对内存占用仍然很低

3）线程安全，没有加锁、解锁、死锁这些问题

nodejs的核心之一就是非阻塞的异步IO
虽然nodejs是单线程的，但它的IO操作是多线程的，多个IO请求会创建多个libeio线程（最多4个），使通常情况的IO操作性能得到提高。

如何解决高并发？

apache，是一个请求一个线程。
异步IO和事件驱动（回调函数）
Apache是专门用了提供HTTP服务的，以及相关配置的（例如虚拟主机、URL转发等等） 
Tomcat是Apache组织在符合J2EE的JSP、Servlet标准下开发的一个JSP服务器 

数据库查询都以非阻塞的方式执行，

在事件处理过程中，它会智能地将一些涉及到IO、网络通信等耗时比较长的操作，交由worker threads去执行，

对CPU要求比较高的CPU密集型任务多的,nodejs不适合

NodeJS适合运用在高并发、I/O密集、少量业务逻辑的场景


既然客户端JavaScript是单线程执行的，回调函数是谁调用的呢？答案很简单，JavaScript的宿主环境——浏览器，也就是说虽然JavaScript是单线程执行的，但浏览器是多线程的，负责调度管理JavaScript代码，让它们在恰当的时机执行。

所以我们所说的node.js单线程，是指node.js并没有给我们创建一个线程的能力，所有我们自己写的代码都是单线程执行的，在同一时间内，只能执行我们写的一句代码。但宿主环境node.js并不是单线程的，它会维护一个执行队列，循环检测，调度JavaScript线程来执行。因此单线程执行和并发操作并不冲突。

 nodejs内置http模块，不需要apache
使用node.js可以轻易地搭建一个网站和服务器组合，而不用想使用PHP还需要额外的Apache服务器，通过特有模块或CGI调用才能将PHP脚本结果返回给用户。


最后在唠叨一句，单线程执行是指我们写的JavaScript语句在同一时刻只能执行一句，而不是node.js是单线程，其实我们的异步I/O及事件循环都是另外线程在做。


node 带环境变量启动
PORT=5380 NODE_DEBUG=redis FAUNA_WEB_CONFIG_PATH=/Users/zhengbj/git/fauna-web/config.json npm start
environment variables node ***


cannot read property * of undefined     对象中找不到这个属性


nodejs 项目登录
服务器启动， 客户端（浏览器）请求nodejs， nodejs查看cookie中有没有connect.sid，如果有和服务器存储的seesion比对，如果有



1.nodejs extend 
var opts = extend({}, sqRequest.default  , opt);
extend(dest,src1,src2…)
后面的参数如果和前面的参数存在相同的名称，那么后面的会覆盖前面的参数值。