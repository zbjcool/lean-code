Weex把JS Framework内置在SDK里面，用来解析从服务器上下载的JS Bundle，这样也减少了每个JS Bundle的体积，JS Framework解析完成以后会输出Json格式的Virtual DOM，客户端Native只需要专心负责 Virtual DOM 的解析和布局、UI 渲染
首先JSFramework的初始化只会在App启动时初始化一次
component: 内置组件
module：暴露native给js调用, 是一组能被JS Framework调用的API.
自定义事件：
```js
// 假设提供mapLoaded事件
<template>
    <div>
        <map style="width:200px;height:200px" @mapLoaded="onMapLoaded"></map>
    </div>
</template>
```
```objective-c
// 触发事件
- (void)mapViewDidFinishLoadingMap:(MKMapView *)mapView {
    if (_mapLoaded) {
        [self fireEvent:@"mapLoaded" params:@{@"customKey":@"customValue"} domChanges:nil];
    }
}
```

我们可以把Weex看做是一个提供了基础套件的UI渲染库
weex开发的web端

Weex 是一个跨平台解决方案，Web 平台只是其一种运行环境，除此之外还可以在 Android 和 iOS 客户端中运行。原生开发平台和 Web 平台之间的差异，在功能和开发体验上都有一些差异。


handler
看看Weex需要你实现的handler中有哪些你要用到的，并实现它们。

module

暴露native给js调用, 是一组能被JS Framework调用的API.

比如刷新

js调用native的module
var eventModule = weex.requireModule(’event’);
eventModule.refresh();


native接受到刷新事件,

-(void)refresh{
    [[NSNotificationCenter defaultCenter] postNotificationName:AppNotiWXPageRefresh object:nil];}


发送一个通知给wxviewcontroller

- (void)_notificationRefreshPageInstance:(NSNotification *)notification {
    [[WXSDKManager bridgeMgr] fireEvent:_instance.instanceId ref:WX_SDK_ROOT_REF type:@"needrefresh" params:nil domChanges:nil];}
wxviewcontroller 接受到通知，
navigation 操作能力，扫码，登录，登出，reload，refresh


fireEvent
native调用js

<template>  
<div class="wrapper”      
    @initparams="mInitParam”      
    @needrefresh="mRefresh”  
>
 </div></template>
methods: {
mInitParam: function () {
    this.mGetNativeInfo();  
},  
mRefresh: function () {
    this.mInit(); 
},
}


1.weex xxx.vue  可以直接运行某文件


2.weex 初始化工程
weex-toolkit是一个很好的工具供我们工程构建。首先，第一步是安装该工具：
$npm install -g weex-toolkit   



工作原理：

转换：在weex中写<template>, <style> 和 <script>标签，使用transformer然后把这些标签转换为JS Bundle用于部署，使用weex-vue-loader
Weex we 文件 --------------前端(we源码)
↓ (转换) ------------------前端(构建过程)
JS Bundle -----------------前端(JS Bundle代码)
↓ (部署) ------------------服务器
在服务器上的JS bundle ----服务器
↓ (编译) ------------------ 客户端(JS引擎)
虚拟 DOM 树 --------------- 客户端(Weex JS Framework)
↓ (渲染) ------------------ 客户端(渲染引擎)
Native视图 --------------- 客户端(渲染引擎)
编译：运行于客户端的的JS框架，管理着Weex实例的运行。Weex实例由JS Bundle创建并构建起虚拟DOM树. 另外，它发送/接受 Native 渲染层产生的系统调用，从而间接的响应用户交互。
渲染：Native引擎: 在不同的端上，有着不同的实现: iOS/Android/HTML5. 他们有着共同的组件设计, 模块API 和渲染效果. 所以他们能配合同样的 JS Framework 和 JS Bundle工作。


* callNative 是一个由native代码实现的函数, 它被JS代码调用并向native发送指令,例如 rendering, networking, authorizing和其他客户端侧的 toast 等API.
* callJS 是一个由JS实现的函数, 它用于Native向JS发送指令. 目前这些指令由用户交互和Native的回调函数组成.

Weex 的 JS Framework 是一个 MVVM，即 Model-View-ViewModel 框架。他会自动监听数据的变化，并通过 {{字段名}} 的语法把数据和视图中所展示的内容自动绑定起来。当数据被改写的时候，视图会自动根据数据的变化而发生相应的变化。


传给JS Framework，JS Framework解析完成以后会输出Json格式的Virtual DOM，客户端Native只需要专心负责 Virtual DOM 的解析和布局、UI 渲染。

[weex问题jira列表](https://issues.apache.org/jira/projects/WEEX/issues/WEEX-646?filter=allopenissues)

工作原理
在终端（Web端、iOS端或Android端）中，由Weex的JS Framework 接收和执行JS Bundle代码，并且执行数据绑定、模板编译等操作，然后输出JSON 格式的 Virtual DOM；JS Framework发送渲染指令给Native ，提供 callNative 和 callJS 接口，方便 JS Framework 和 Native 的通信。