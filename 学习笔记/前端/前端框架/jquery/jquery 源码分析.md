匿名函数立即执行。(function (){ })();
为什么这么做，函数内部的变量，方法。外部无法访问。
(function (){
   var a = 10;
   function a (){
        alert(a);
   }
})();

(function () {
     (21, 94) 定义了一些变量和函数  jQuery = function (){};
     (96, 283) 给JQ对象，添加一些方法和属性
     (285, 347) extend jq的继承方法
     (349， 817) jQuery.extend() 扩展一些工具方法
     (877, 2856) sizzle:复杂选择器的实现
     (2888, 3042) callbacks: 回调对象：函数的统一管理
     (3043, 3183) deferred: 延迟对象：对异步的统一管理
     (3184, 3295) support: 功能检测
     (3308, 3652) data() : 数据缓存
     (3653,3797) queue() : 队列管理
     (3780,4299) attr() prop() val() addClass()等，对元素属性的操作
     (4300, 5128)  on()，trigger() ：事件操作的相关方法
     (5140,6057)DOM操作：添加 删除 获取 包装 筛选
     (6058,6620)css() 
     (6621,7854) 提交的数据和ajax()
     (7855,8584) animate():
     (8585,8792) offset 位置和尺寸
     (8804,8821) jq支持模块化的
     (8826) windows.jQuery = window.$ = jQuery;
})();


        function fn1(){alert(1)};
        function fn2(){alert(2)};

        var callbacks = $.Callbacks();
        callbacks.add(fn1);
        callbacks.add(fn2);

        callbacks.fire();//1,2

         callbacks.remove(fn2);
          callbacks.fire();//1