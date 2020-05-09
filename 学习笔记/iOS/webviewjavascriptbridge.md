在UIWebView中，native有直接调用js的方法,js没有直接调用native的方法
evaluateJavaScript
虽然无法直接调用native代码，但是iOS的接口中还是设计了可以通过间接的方式传递js调用的消息

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType;
这个方法，可以每次在UIWebView进行重定向URL的时候，进行触发，只要把一个js调用native的方法包装成一个重定向URL,就可以在本地接收到相应的方法。

native 端
首先是在页面加载完成的时候，native会注入一段js代码：