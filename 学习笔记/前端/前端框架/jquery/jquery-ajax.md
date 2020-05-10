只有success
$.get
$.post  
 
$.load 获取string
$.getJSON 获取json
$.getScript  获取js脚本


事件处理

html事件处理
直接添加到HTML结构中
<button id=“btn1”>按钮</button>
<script>
function demo(){
     
}
</script>

dom0级事件处理
优点是只要改动一处，
<script>
var btn1 = document.getElementById(“btn1”);
btn1.onclick = function (){alert(“sdf1”)};
btn1.onclick = function (){alert(“sdf2”)};//会被覆盖
btn1.onclick = null;
</script>

把一个函数赋值给一个事件处理程序属性
dom2级事件处理
添加多个不会被覆盖
addEventLisener(“事件名”, “事件处理函数”, “布尔值")
true：事件捕获
false： 事件冒泡
removeEventListener();
