

## obj 类底层知识
```objective-c
NSObject.h
@interface NSObject <NSObject> {  
  Class isa  OBJC_ISA_AVAILABILITY;  
}
```
NSObject  有一个isa的属性，类型是Class


<objc.h>
```objective-c
typedef struct objc_class *Class;  
typedef struct objc_object {  
  Class isa;  
} *id;
```
Class 是一个 objc_class 结构类型的指针,id是一个 objc_object 结构类型的指针.

<runtime.h>
```objective-c
struct objc_class {  
  Class isa;  
  Class super_class;  
  const charchar *name;  
  long version;  
  long info;  
  long instance_size;  
  struct objc_ivar_list *ivars;  
  struct objc_method_list **methodLists;  
  struct objc_cache *cache;  
  struct objc_protocol_list *protocols;  
} OBJC2_UNAVAILABLE;  
```

当我们向一个对象发送消息时，runtime会在这个对象所属的这个类的方法列表中查找方法；而向一个类发送消息时，会在这个类的meta-class的方法列表中查找。

meta-class之所以重要，是因为它存储着一个类的所有类方法。每个类都会有一个单独的meta-class，因为每个类的类方法基本不可能完全相同。

isa

是一个 Objective-C Class 类型的指针. 实例对象有个isa的属性,指向Class, 而Class里也有个isa的属性, 指向meteClass(
meta-class是一个类对象的类。
). 这里就有个点, 在Objective-C中任何的类定义都是对象.类自身也是一个对象
super_class

指向该类的父类, 如果该类已经是最顶层的根类(如 NSObject 或 NSProxy),那么 super_class 就为 NULL.
name
类名字
ivars

成员变量的数组
methodLists

方法定义的数组

## Runtime（运行时）
#import <objc/runtime.h> 

运行时常用判断方法
// 当前对象是否这个类或其子类的实例
- (BOOL)isKindOfClass:(Class)aClass;
// 当前对象是否是这个类的实例
- (BOOL)isMemberOfClass:(Class)aClass;
// 当前对象是否遵守这个协议
- (BOOL)conformsToProtocol:(Protocol *)aProtocol;
// 当前对象是否实现这个方法
- (BOOL)respondsToSelector:(SEL)aSelector;
[self isKindOfClass:NSClassFromString(@“UIVIew”)]
我们可以通过类名/方法名反射得到相应的类和方法：
Class class = NSClassFromString("UIViewController");
id viewController = [[class alloc] init];
SEL selector = NSSelectorFromString("viewDidLoad");
[viewController performSelector:selector];
例子：
    for (__strong UIView *possibleKeyboard in [keyboardWindow subviews]) {
        if ([possibleKeyboard isKindOfClass:NSClassFromString(@"UIPeripheralHostView")] || [possibleKeyboard isKindOfClass:NSClassFromString(@"UIKeyboard")]) {
            return CGRectGetHeight(possibleKeyboard.bounds);
        } else if ([possibleKeyboard isKindOfClass:NSClassFromString(@"UIInputSetContainerView")]) {
            for (__strong UIView *possibleKeyboardSubview in [possibleKeyboard subviews]) {
                if ([possibleKeyboardSubview isKindOfClass:NSClassFromString(@"UIInputSetHostView")]) {
                    return CGRectGetHeight(possibleKeyboardSubview.bounds);
                }
            }
        }
    }
//我们有时候需要获取某个view下面的subviews 

正常来说，
id myObj = [[NSClassFromString(@"MySpecialClass") alloc] init];
和
id myObj = [[MySpecialClass alloc] init];
是一样的。但是，如果你的程序中并不存在MySpecialClass这个类，下面的写法会出错，而上面的写法只是返回一个空对象而已。
因此，在某些情况下，可以使用NSClassFromString来进行你不确定的类的初始化。

不需要使用import，因为类是动态加载的，只要存在就可以加载。因此如果你的toolchain中没有某个类的头文件定义，而你确信这个类是可以用的，那么也可以用这种方法。


(1)、获取对象的类名 object_getClassName(id obj)
- (void)beginLogPageView
{
    [MobClick beginLogPageView:(self.title?:[NSString stringWithUTF8String:object_getClassName(self)])];
//    NSLog(@"访问路径进入：%@", (self.title?:[NSString stringWithUTF8String:object_getClassName(self)]));
}

- (void)endLogPageView
{
    [MobClick endLogPageView:(self.title?:[NSString stringWithUTF8String:object_getClassName(self)])];
//    NSLog(@"访问路径退出：%@", (self.title?:[NSString stringWithUTF8String:object_getClassName(self)]));
}}
object_getClassName(self) 埋点统计的时候记录进入页面和退出页面的时间，需要获取进入该viewController类名

(2) 给类添加一个方法 BOOL class_addMethod(Class cls,SEL name,IMP imp, const char *types)
(3) 获取一个类所有方法，属性 
    u_int count;
    Method* methods= class_copyMethodList([UIViewController class], &count);
  u_int count;
    objc_property_t *properties=class_copyPropertyList([UIViewControllerclass], &count);
 Selector相当于一个方法的id；IMP是方法的实现。这样分开的一个便利之处是selector和IMP之间的对应关系可以被改变。比如一个 IMP 可以有多个 selectors 指向它。
class_getInstanceMethod(Class cls, SEL name)
而 Method Swizzling 可以交换两个方法的实现。
你可以重写某个方法而不用继承，同时还可以调用原先的实现。
+ (void)load {
    // is this the best solution?
    method_exchangeImplementations(class_getInstanceMethod(self.class, NSSelectorFromString(@"dealloc")),
                                   class_getInstanceMethod(self.class, @selector(swizzledDealloc)));
}
+load
Swizzling 的处理，在类的 +load 方法中完成。
因为 +load 方法会在类被添加到 OC 运行时执行，保证了 Swizzling 方法的及时处理。
本质是交换imp

Method Swizzling 非常强大，主要作用有 * 修改 iOS 系统类库的方法 * 动态添加、修改方法，修复线上 bug（如果 Apple 官方允许的话）
使用例子：解决ios7下多次push导致：Can't add self as subview



Category是Objective-C中常用的语法特性，通过它可以很方便的为已有的类来添加函数。但是Category不允许为已有的类添加新的属性或者成员变量。    




void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy);
id objc_getAssociatedObject(id object, const void *key);
void objc_removeAssociatedObjects(id object);
objc_removeAssociatedObjects 函数我们一般是用不上的，因为这个函数会移除一个对象的所有关联对象，将该对象恢复成“原始”状态。这样做就很有可能把别人添加的关联对象也一并移除，这并不是我们所希望的。所以一般的做法是通过给 objc_setAssociatedObject 函数传入 nil 来移除某个已有的关联对象。


//NSObject+IndieBandName.h
@interface NSObject (IndieBandName)
@property (nonatomic, strong) NSString *indieBandName;

// NSObject+IndieBandName.m   
#import "NSObject+Extension.h"
#import <objc/runtime.h>
static const void *IndieBandNameKey = &IndieBandNameKey;   
@implementation NSObject (IndieBandName)
@dynamic indieBandName;

- (NSString *)indieBandName {
    return objc_getAssociatedObject(self, IndieBandNameKey);
}

- (void)setIndieBandName:(NSString *)indieBandName{
    objc_setAssociatedObject(self, IndieBandNameKey, indieBandName, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
使用场景：为现有的类添加公有属性；
关联对象与被关联对象本身的存储并没有直接的关系，它是存储在单独的哈希表中的
使用例子：
UIBarButtonItem+Badge
把现有的控件通过自己的扩展


## category 和 继承
继承：
新扩展的方法与原方法同名，但是还需要使用父类的实现。因为使用类别，会覆盖原类的实现，无法访问到原来的方法。
category：
 1）针对系统提供的一些类，例如：NSString,NSArray,NSNumber等类，系统本身不提倡使用继承去扩展方法，因为这些类内部实现对继承有所限制，所以最后使用类别来进行方法扩展。
2）类别支持开发人员针对自己构建的类，把相关的方法分组到多个单独的文件中，对于大型而复杂的类，这有助于提高可维护性，并简化单个源文件的管理。


* tableview卡顿如何优化，其中委托方法调用顺序。
* UIWebView，wkWebView 有哪些 delegate
iOS runTime 概念，使用地方。
iOS多线程，GCD，NSThread，RunLoop。概念，使用的地方。


## 多线程

NSOperation :
    NSMutableURLRequest *storeRequest = [NSMutableURLRequest requestWithURL:storeURL];
    [storeRequest setHTTPMethod:@"POST"];
    [storeRequest setHTTPBody:requestData];
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    [NSURLConnection sendAsynchronousRequest:storeRequest queue:queue
                           completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
                               if (connectionError) {
                                   /* 处理error */
                               } else {
                                   NSError *error;
                                   NSDictionary *jsonResponse = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
                                   if (!jsonResponse) {
                                       /* 处理error */
                                   }else{
                                       /* 处理验证结果 */
                                   }
                               }
                           }];

GCD:
* dispatch_sync(dispatch_queue_t queue, dispatch_block_t block);
用同步的方式执行任务（当前线程中执行）
* dispatch_async(dispatch_queue_t queue, dispatch_block_t block);
用异步的方式执行任务（另起一条线程中执行）
dispatch_async() 调用以后立即返回，dispatch_sync() 调用以后等到block执行完以后才返回，dispatch_sync()会阻塞当前线程。 

线程队列
 dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
  //添加任务到队列执行任务
  //异步函数 具备开启新线程能力
  dispatch_async(queue, ^{
    NSLog(@"下载图片1---%@", [NSThread currentThread]);
  });
  
  dispatch_async(queue, ^{
    NSLog(@"下载图片2---%@", [NSThread currentThread]);
  });
  
  dispatch_async(queue, ^{
    NSLog(@"下载图片3---%@", [NSThread currentThread]);
  });
