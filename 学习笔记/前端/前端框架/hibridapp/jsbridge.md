# JSBridge

    构建 Native 和非 Native 间消息通信的通道，而且是 双向通信的通道。
    
JavaScript 是运行在一个单独的 JS Context 中（例如，WebView 的 Webkit 引擎、JSCore）。由于这些 Context 与原生运行环境的天然隔离，我们可以将这种情况与 RPC（Remote Procedure Call，远程过程调用）通信进行类比，将 Native 与 JavaScript 的每次互相调用看做一次 RPC 调用。


* native调用js方法：
[webView stringByEvaluatingJavaScriptFromString:@"Math.random();"];
调用了window下的一个对象，只要暴露一个对象如JSBridge对native调用就好了
因为 JavaScript 的方法必须在全局的 window 上。
* js调用native方法
UIWebView有个特性：在UIWebView内发起的所有网络请求，都可以通过delegate函数在Native层得到通知。
我们就可以在UIWebView内发起一个自定义的网络请求，通常是这样的格式：jsbridge://methodName?param1=value1&param2=value2

于是在UIWebView的delegate函数中，我们只要发现是jsbridge://开头的地址，就不进行内容的加载，转而执行相应的调用逻辑。
发起这样一个网络请求有两种方式：1. 通过localtion.href；2. 通过iframe方式；
通过location.href有个问题，就是如果我们连续多次修改window.location.href的值，在Native层只能接收到最后一次请求，前面的请求都会被忽略掉。
iframe 方式：通过定义自己的 url schema；把 callbackid 和对应的 callback 拼成对象，放在 windows 上，把参数和 callbackid 放在 url 上，通过 iframe。src 上，append 到 body