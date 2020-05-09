JSPatch 能做到通过 JS 调用和改写 OC 方法最根本的原因是 Objective-C 是动态语言，OC 上所有方法的调用/类的生成都通过 Objective-C Runtime 在运行时进行。

JS 传递字符串给 OC，OC 通过 Runtime 接口调用和替换 OC 方法
require('UIView')
var view = UIView.alloc().init()
view.setBackgroundColor(require('UIColor').grayColor())
view.setAlpha(0.5)
在 OC 执行 JS 脚本前，通过正则把所有方法调用都改成调用 __c() 函数，再执行这个 JS 脚本，做到了类似 OC/Lua/Ruby 等的消息转发机制：