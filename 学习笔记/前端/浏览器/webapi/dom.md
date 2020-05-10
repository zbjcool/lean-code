## dom1、dom2、dom3
### dom1
 DOM1 级由两个模块组成:DOM 核心(DOM Core)和 DOM HTML。
 
 DOM1 级的目标主要是映射文档的结构
 
### dom2
 DOM 视图(DOM Views):定义了跟踪不同文档(例如，应用 CSS 之前和之后的文档)视图的
接口;
 DOM 事件(DOM Events):定义了事件和事件处理的接口;
 DOM 样式(DOM Style):定义了基于 CSS 为元素应用样式的接口;
 DOM 遍历和范围(DOM Traversal and Range):定义了遍历和操作文档树的接口。

### dom3

DOM3 级则进一步扩展了 DOM，引入了以统一方式加载和保存文档的方法——在 DOM 加载和保 存(DOM Load and Save)模块中定义;新增了验证文档的方法——在 DOM 验证(DOM Validation)模块中定义
## 概况
 操作文档内容
 
 文档树，文档树包含标签，表示文本字符串的节点
 根部是 document 节点
 html 元素是element 节点
 文本是text 节点
 document，element，text 是 node 对象的子类
 
 htmlDocument
 HtmlElement

 ## 选取文档元素
<input id="inputId" name="myInput" type="text" size="20" /><br />
### 通过 id 选取元素
id 必须是唯一的，如果相同就选取第一个
getElementById('inputId')
返回对拥有指定 id 的第一个对象的引用。
### 通过 name 选取元素
返回 nodeList
getElementsByName('myInput')
返回带有指定名称的对象集合。
### 通过tag选取元素
方法可返回带有指定标签名的对象的集合。返回 nodeList
getElementsByTagName(‘input')

### 通过 class选取元素
getElementsByClassName()
饭户籍 HTMLcollection 列表

### 通过选择器选取元素

querySelector() 
方法仅仅返回匹配指定选择器的第一个元素。如果你需要返回所有的元素，请使用 querySelectorAll() 方法替代。

css 的选择器参考 css 的文档

#idName div .classname [name="x"]
组合
span.fatal.error
span[name="aa"].warning
文档结构
#log span   id是 log 的所有 span
#log>span 子元素
body>h1:first-child

div,#log 所有的 div 和#log

document.querySelector("#demo”); id
document.querySelector("[name=myInput]”); 属性
document.querySelector(“.myInputClass”); 类
document.querySelector(“input”); 标签
 
## getElement和 querySelector()
query选择符选出来的元素及元素数组是静态的，而getElement这种方法选出的元素是动态的。
HTML代码
```js
    <ul>
        <li>111</li>
        <li>222</li>
        <li>333</li>
    </ul>
        var ul=document.querySelector('ul');
        var list=ul.querySelectorAll('li');
        for(var i=0;i<list.length;i++){
            ul.appendChild(document.createElement('li'));
        }//这个时候就创建了3个新的li，添加在ul列表上。
        console.log(list.length) //输出的结果仍然是3，不是此时li的数量6
反观getElement方法
        var ul=document.getElementsByTagName('ul')[0];
        var list=ul.getElementsByTagName('li');
        for(var i=0;i<5;i++){
            ul.appendChild(document.createElement('li'));
        }   
        console.log(list.length)//此时输出的结果就是3+5=8
```
getElement
速度快
返回的 NodeList 对象都是个实时集合。意思是说，如果文档中的节点树发生变化，则已经存在的 NodeList 对象也可能会变化。例如，Node.childNodes 是实时的：
querySelectorAll 
返回的 NodeList 是一个静态集合，也就意味着随后对文档对象模型的任何改动都不会影响集合的内容。

特别是当你选择如何遍历 NodeList 中所有项，或缓存列表长度的时候，最好牢记这种区分。


### element 对象
style
className


