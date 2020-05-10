[CSS使用技巧](http://www.ruanyifeng.com/blog/2010/03/css_cookbook.html)
内空间 padding border margin 


box-sizing:  // 盒模型的计算模式
border-box // 宽度高度包含padding和border，指定了
padding-box // width包含了padding
content-box // width只是包含content的width


border

* border-radius
* box-shadow
* border-image

background:
* background-size
* background-origin : border-box, content-box，padding-box

text:
* text-shadow
* word-wrap : 自动换行


transform:
转换是使元素改变形状、尺寸和位置的一种效果。
2D
* translate(xDis,yDis) translateX(xDis) translateY(yDis)
* rotate(angle) // 10deg -10deg
* scale(xMultiple,yMultiple) scaleX(xMultiple) scaleY(xMultiple)
* skew()
* matrix()

3D
rotateX rotateY rotateZ

animation
@keyframes myfirst
{
from {background: red;}
to {background: yellow;}
}

div
{
animation: myfirst 5s;
//或者 animation-name: myfirst
// animaton-duration: 5s
}

backface-visibility



animation 属性：
animation
所有动画属性的简写属性，除了 animation-play-state 属性。 
3 
animation-name
规定 @keyframes 动画的名称。 
3 
animation-duration
规定动画完成一个周期所花费的秒或毫秒。默认是 0。 
3 
animation-timing-function
规定动画的速度曲线。默认是 "ease"。 
3 
animation-delay
规定动画何时开始。默认是 0。 
3 
animation-iteration-count
规定动画被播放的次数。默认是 1。 
3 
animation-direction
规定动画是否在下一周期逆向地播放。默认是 "normal"。 
3 
animation-play-state
规定动画是否正在运行或暂停。默认是 "running"。 
3 
animation-fill-mode
规定对象动画时间之外的状态。 
3 

@keyframes myfirst
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

@keyframes myfirst
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}


类似天猫超市翻牌


css 选择器
.class 
#id
星号：所有元素
element ： 选择所有element标签
element,element  div,p 选择所有<div>和<p>元素
element element div p 选择所有<div>元素内部的所有<p>
element>element div>p 选择所有父元素为<div>的所有<p>元素
element+element div+div  选择所有div元素之后的所有div
[attribute]
[target] 
选择带有 target 属性所有元素。 
2 
[attribute=value]
[target=_blank] 
选择 target="_blank" 的所有元素。 
2 
[attribute~=value]
[title~=flower] 
选择 title 属性包含单词 "flower" 的所有元素。 


[attribute|=value]
[lang|=en] 
选择 lang 属性值以 "en" 开头的所有元素。 




css:
box-sizing:
content-box: 默认，width就是宽度
border-box:width包含border和padding
position，
absolute:脱离文档流， 相对于 static 定位以外的第一个父元素进行定位。
fixed: 生成绝对定位的元素，相对于浏览器窗口进行定位。
relative: 对象不可层叠、不脱离文档流，生成相对定位的元素，相对于其正常位置进行定位。
父是absolute，子是absolute,  子的定位会收到父的影响
父是absolute，子是relative  子的定位会收到父的影响
父是relative，子是absolute 相对父的绝对定位
父是relative，子是relative 相对于父的定位


200 成功
300 是重定向状态码
400 服务器无法处理请求
认证不通过，请求参数错误，
weex:坑
不支持写border: 1 solid #ff0000;
不支持v-show，display:none
支持的css太少
下拉刷新，上拉加载



position:absolute relative 区别
absolute：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
relative：生成相对定位的元素，相对于其正常位置进行定位。

两者最核心的区别在于：**absolute不受父元素里的其他元素影响，而relative会受到父元素里的其他元素影响。