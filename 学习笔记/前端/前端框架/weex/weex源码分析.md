
1、程序启动
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // 在这里进行初始化SDK
    [self initWeexSDK];
    return YES;
}
程序起来在这里进行初始化

2、初始化
- (void)initWeexSDK
{
    //WXAppConfiguration是一个用来记录App配置信息的单例对象。
    [WXAppConfiguration setAppGroup:@"AliApp"];
    [WXAppConfiguration setAppName:@"WeexDemo"];
    [WXAppConfiguration setExternalUserAgent:@"ExternalUA"];
        //初始化SDK
    [WXSDKEngine initSDKEnvironment];

    [WXSDKEngine registerHandler:[WXImgLoaderDefaultImpl new] withProtocol:@protocol(WXImgLoaderProtocol)];
    [WXSDKEngine registerHandler:[WXEventModule new] withProtocol:@protocol(WXEventModuleProtocol)];

    [WXSDKEngine registerComponent:@"select" withClass:NSClassFromString(@"WXSelectComponent")];
    [WXSDKEngine registerModule:@"event" withClass:[WXEventModule class]];
    [WXSDKEngine registerModule:@"syncTest" withClass:[WXSyncTestModule class]];
}

1、初始化SDK
+ (void)initSDKEnvironment
{
    //WXMonitor监视器记录状态
    //#define WX_MONITOR_PERF_START(tag) [WXMonitor performancePoint:tag                              willStartWithInstance:nil];
//WXMonitor 存储了线程安全的字典：static WXThreadSafeMutableDictionary *globalPerformanceDict;

//在这个字典初始化的时候会初始化一个queue。每次生成一次WXThreadSafeMutableDictionary，就会有一个与之内存地址向对应的Concurrent的queue相对应。这个queue就保证了线程安全。
//WXMonitor在整个Weex里面担任的职责是记录下各个操作的tag值和记录成功和失败的原因。WXMonitor封装了各种宏来方便方法的调用。
    WX_MONITOR_PERF_START(WXPTInitalize)
    WX_MONITOR_PERF_START(WXPTInitalizeSync)

    // 加载本地的main.js
    NSString *filePath = [[NSBundle bundleForClass:self] pathForResource:@"main" ofType:@"js"];
    NSString *script = [NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];

    // 初始化SDK环境
    //main.js 就是js framework
    [WXSDKEngine initSDKEnvironment:script];

    //WXMonitor监视器记录状态
    WX_MONITOR_PERF_END(WXPTInitalizeSync)
}
//WXSDKEngine的初始化就是整个SDK初始化的关键。
+ (void)initSDKEnvironment:(NSString *)script
{
    if (!script || script.length <= 0) {
        WX_MONITOR_FAIL(WXMTJSFramework, WX_ERR_JSFRAMEWORK_LOAD, @"framework loading is failure!");
        return;
    }
    // 注册Components，Modules，Handlers
    [self registerDefaults];

    // 执行JsFramework
    [[WXSDKManager bridgeMgr] executeJsFramework:script];
}
注册组件


+ (void)registerComponent:(NSString *)name withClass:(Class)clazz withProperties:(NSDictionary *)properties
{
    if (!name || !clazz) {
        return;
    }
    WXAssert(name && clazz, @"Fail to register the component, please check if the parameters are correct ！");

    // 1.WXComponentFactory注册组件的方法，WXComponentFactory是一个单例。
    //在WXComponentFactory中，_componentConfigs会存储所有的组件配置，注册的过程也是生成_componentConfigs的过程。
    [WXComponentFactory registerComponent:name withClass:clazz withPros:properties];
    // 2.遍历出所有异步的方法
    NSMutableDictionary *dict = [WXComponentFactory componentMethodMapsWithName:name];
    dict[@"type"] = name;

    // 3.把组件注册到WXBridgeManager中
    if (properties) {
        NSMutableDictionary *props = [properties mutableCopy];
        if ([dict[@"methods"] count]) {
            [props addEntriesFromDictionary:dict];
        }
//WXBridgeManager就是用来和JS交互的Bridge。
        [[WXSDKManager bridgeMgr] registerComponents:@[props]];
    } else {
        [[WXSDKManager bridgeMgr] registerComponents:@[dict]];
    }
}
注册组件全部都是通过WXComponentFactory完成注册的。WXComponentFactory是一个单例。
- (void)registerComponent:(NSString *)name withClass:(Class)clazz withPros:(NSDictionary *)pros
{
    WXAssert(name && clazz, @"name or clazz must not be nil for registering component.");

    WXComponentConfig *config = nil;
    [_configLock lock];
    config = [_componentConfigs objectForKey:name];

    // 如果组件已经注册过，会提示重复注册，并且覆盖原先的注册行为
    if(config){
        WXLogInfo(@"Overrider component name:%@ class:%@, to name:%@ class:%@",
                  config.name, config.class, name, clazz);
    }

    config = [[WXComponentConfig alloc] initWithName:name class:NSStringFromClass(clazz) pros:pros];
    [_componentConfigs setValue:config forKey:name];

    // 注册类方法
//注册组件暴露出来的方法，WX_EXPORT_METHOD，class_copyMethodList获取所有类方法
//如果名字里面不带wx_export_method_前缀的方法，那么都不算是暴露出来的方法，直接continue
    [config registerMethods];
    [_configLock unlock];
}

这就完成了组件注册的第一步，完成了注册配置WXComponentConfig。
第二步：这里依旧是调用了WXComponentFactory的方法_componentMethodMapsWithName:。这里就是遍历出异步方法，并放入字典中，返回异步方法的字典。
注册组件的最后一步会在JSFrame中注册组件。
在WXSDKManager里面会强持有一个WXBridgeManager。
WXBridgeManager中会弱引用WXSDKInstance实例，
WXBridgeManager里面最重要的一个属性就是WXBridgeContext。
在WXBridgeContext中强持有了一个jsBridge。这个就是用来和js进行交互的Bridge。
在WXBridgeContext中注册组件，其实调用的是main.js的方法"registerComponents"。

- (void)registerComponents:(NSArray *)components
{
    WXAssertBridgeThread();
    if(!components) return;
    [self callJSMethod:@"registerComponents" args:@[components]];
}



执行jsframework
    [[WXSDKManager bridgeMgr] executeJsFramework:script];
WXSDKManager会调用WXBridgeManager去执行SDK里面的main.js文件。
- (void)executeJsFramework:(NSString *)script
{
    if (!script) return;

    __weak typeof(self) weakSelf = self;
    WXPerformBlockOnBridgeThread(^(){
        [weakSelf.bridgeCtx executeJsFramework:script];
    });
}
WXBridgeManager通过WXBridgeContext调用executeJsFramework:方法。这里方法调用也是在子线程中进行的。

- (void)executeJsFramework:(NSString *)script
{
    WXAssertBridgeThread();
    WXAssertParam(script);

    WX_MONITOR_PERF_START(WXPTFrameworkExecute);

    [self.jsBridge executeJSFramework:script];

    WX_MONITOR_PERF_END(WXPTFrameworkExecute);

    if ([self.jsBridge exception]) {
        NSString *message = [NSString stringWithFormat:@"JSFramework executes error: %@", [self.jsBridge exception]];
        WX_MONITOR_FAIL(WXMTJSFramework, WX_ERR_JSFRAMEWORK_EXECUTE, message);
    } else {
        WX_MONITOR_SUCCESS(WXMTJSFramework);
        // 至此JSFramework算完全加载完成了
        self.frameworkLoadFinished = YES;

        // 执行所有注册的JsService
        [self executeAllJsService];

         // 获取JSFramework版本号
        JSValue *frameworkVersion = [self.jsBridge callJSMethod:@"getJSFMVersion" args:nil];
        if (frameworkVersion && [frameworkVersion isString]) {
            // 把版本号存入WXAppConfiguration中
            [WXAppConfiguration setJSFrameworkVersion:[frameworkVersion toString]];
        }

        // 执行之前缓存在_methodQueue数组里面的所有方法
        for (NSDictionary *method in _methodQueue) {
            [self callJSMethod:method[@"method"] args:method[@"args"]];
        }

        [_methodQueue removeAllObjects];

        // 至此，初始化工作算完成了。
        WX_MONITOR_PERF_END(WXPTInitalize);
    };
}

- (void)executeJSFramework:(NSString *)frameworkScript
{
    WXAssertParam(frameworkScript);
    if (WX_SYS_VERSION_GREATER_THAN_OR_EQUAL_TO(@"8.0")) {
        [_jsContext evaluateScript:frameworkScript withSourceURL:[NSURL URLWithString:@"main.js"]];
    }else{
        [_jsContext evaluateScript:frameworkScript];
    }
}

加载JSFramework的核心代码在这里，通过JSContext执行evaluateScript:来加载JSFramework。由于这里并没有返回值，所以加载的JSFramework的目的仅仅是声明了里面的所有方法，并没有调用。这也符合OC加载其他Framework的过程，加载只是加载到内存中，Framework里面的方法可以随时被调用，而不是一加载就调用其所有的方法。



