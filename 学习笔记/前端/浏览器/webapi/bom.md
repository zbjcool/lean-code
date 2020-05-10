浏览器对象，browser对象
window属性和方法都是全局属性和全局方法
## Location：


http://www.w3school.com.cn:80/tiy/t.asp?f=hdom_loc_search#part2

protocol == http:

hostname == www.w3school.com.cn

host = www.w3school.com.cn:80

port == 80

pathname == /tiy/t.asp

search == ?f=hdom_loc_search

hash == #part2


href==protocol//hostname:port+ pathname+search+hash
host = hostname:port


### 载入新的文档
assign()
绝对地址
相对地址：修改在当前地址的最后一个path

replace():
载入新文档，会从浏览历史中把当前文档删除

reload()

或者修改 location 属性
location = "http://www.baidu.com"
location = "xxx.html"
location = '#top'

location.search = "?page=2"





## setTimeout() setInterval()

var id = setTimeout(callback,sec)
clearTimeout(id)

如果是0秒，指定的函数也不会立即执行，会放入队列，等待前面的任务执行后，再调用它
## history
length 属性表示浏览历史列表中的元素数量
back()
forwrad
go(-2)
浏览历史管理

### 解决方案
1. jQuery的 history 插件
2. RSH
## navigator
桌面相关信息
userAgent

## screen 对象

width,height,

## 对话框
alert(),prompt(),confirm(),showModalDialog()

## onerror

## doucument

## 作为 window 对象属性的文档属性

html 文档中使用 id 属性会成为全局属性
```js
<div id="okay"></div>
window.okay
window['okay']
```

## open()
window.open()
打开新的浏览器窗口
window.close()