HTML元素选取
HTML元素操作
css操作
html事件函数
js特效和动画
html dom遍历和修改
ajax
utilities


div[class^='download-‘]{}
选取属性class，‘download-’打头的
## jQuery  延迟对象 Deferred
setTimeout (function(){
     alert(2);
},1000);
alert(1);//先执行1， 在执行2


var did = $.Deferred();
setTimeout (function(){
     alert(1);
          dfd.resolve();//调用done方法
},1000);

//先把完成的回调存起来
dfd.done(function(){
     alert(2);//先执行1， 在执行2
});

用deferred 方法

