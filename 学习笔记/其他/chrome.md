ctr + tab 向后切换tab
ctr + shift + tab 向前切换tab



## Chrome 中查看内存快照
首先我们在控制台运行这样一段程序。
```js
function Food(name, type) {
  this.name = name;
  this.type = type;
}
var beef = new Food('beef', 'meat');
```

切换到 Memory 中，点击左侧的小圈圈就可以捕获当前的内存快照。