express

路由分发，请求处理，视图渲染

一个Express应用，从本质上来说，就是一系列中间件的调用。那么，中间件到底是什么呢？其实，一个中间件，就是一个函数。通常情况下，

中间件，处理请求，返回处理

视图渲染 res.render
2、session和cookie 管理
服务器向客户端发送cookie  setCookie connect_sid
浏览器保存cookie，
每次请求发送cookie给浏览器

这意思就是说，当你浏览一个网页时，服务端随机产生一个 1024 比特长的字符串，然后存在你 cookie 中的 connect.sid 字段中。当你下次访问时，cookie 会带有这个字符串，然后浏览器就知道你是上次访问过的某某某，然后从服务器的存储中取出上次记录在你身上的数据。由于字符串是随机产生的，而且位数足够多，所以也不担心有人能够伪造。伪造成功的概率比坐在家里编程时被邻居家的狗突然闯入并咬死的几率还低。

登录就是把user信息存到session中，每次访问，看cookie对应的session中有没有user信息





node的核心理念是事件驱动，用户界面也是事件驱动，服务器响应也是事件驱动。


互联网媒体类型： Content-Type: application/json;charset=UTF-8

post 支持文件上传： multipart/form-data

ajax: application/json
普通： application/x-www-form-urlencoded

如果是xhr请求
X-Requested-With:
XMLHttpRequest

理解模板引擎的关键在于context


hbs注释
{{! xxxxxxxxx}} 如果是服务端模板，不会被传到浏览器
<!— xxxxxxxxx —> 会被传到浏览器

next() 请求在中间件终止了。


应用集群，
请求分布在不同的服务器处理


调试的首要原则是排除，
1.系统化注释掉或禁用代码块
2.编写能被单元测试覆盖的代码
3.分析网络数据流，确定问题是出在客户端还是服务端
4.用版本控制逐次回退，直到问题消失
5.模拟功能已排除复杂子系统的干扰，其实就是简化功能。

控制台日志

js的控制台，交互式编写程序
编写中间件
1.模块直接输出中间件函数
module.exports = function (req,res,next){}
var xxx = require(‘xxxx’);
app.use(‘/‘,xxx);
2.模块输出返回中间件的函数
module.exports = function(config){
    if(!config) config = {};
        return function(req,res,next){}
}
var xxx = require(‘xxx’)({aaa:”aaaa”});
app.use(xxx);

3.如果要输出包含中间件的对象
module.exports = function(config){
    if(!config) config = {};
        return {
                       m1: function(req,res,next){},
                        m2: function(req,res,next){}
                    }
}
var xxx = require(‘xxx’)({aaa:”aaaa”});
app.use(xxx.m1);
app.use(xxx.m2);

4.模块输出对象构造器
function Stuff(config){
    this.config = config || {};
}
Stuff.prototype.m1= function (req,res,next){
}
Stuff.prototype.m1= function (){
    return (function(req,res,next){
        this.config;
        next();
    }).bind(this)
}
module.export





1.req.param() express4.x会被弃用
// /user/tobi for /user/:name 
req.param('name')
// => "tobi"
// ?name=tobi
req.param('name')
// => "tobi"

// POST name=tobi
req.param('name')
// => “tobi”
该方法可以获取
1）express路由器传递的参数;
2）地址栏参数；
3）postt提交的参数，例如表单中input的值， ajax（异步）提交的对象值等。
2.req.params
　　与req.param()方法相比 该属性只能获取 “express路由器传递的参数”,  值得一提的是： 与req.params配合还能在express路由器中玩正则。
先看下简单的req.params 使用：
// GET /user/tj
req.params.name
// => “tj”

var express = require('express');
var app = express();

// 地址栏： localhost:3000/user/tj 
app.get('/user/:name', function(req, res){
  var param = req.params.name
  res.send('hello world' + param); // hello world tj
});
然后看看路由器中神奇的正则使用法,在地址栏输入 localhost:3000/file/javascripts/jquery.js , 而路由中设置了 “/file/*”  时:
var express = require('express');
var app = express();

// 地址栏：localhost:3000/file/javascripts/jquery.js
app.get('/file/*', function(req, res){
  var param = req.params[0];
  res.send(param); //javascripts/jquery.js
});

3.req.query
// GET /search?q=tobi+ferret
req.query.q
// => "tobi ferret"

// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
req.query.order
// => "desc"

req.query.shoe.color
// => "blue"

req.query.shoe.type
// => “converse"
4.req.body

　该属性主要用与post方法时传递参数使用， 用法最为广泛也最为常见， 例子也比较多（写这部分最累了有木有）。需要说明下的是使用该属性时， 得先确认app.js中有没有导入“body-parser”， 该模块在express4.x中已经脱离为独立的模块。示例代码如下：

var app = require('express')();
var bodyParser = require('body-parser');
var multer = require('multer'); 

app.use(bodyParser.json()); // for parsing application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded
app.use(multer()); // for parsing multipart/form-data

app.post('/', function (req, res) {
  console.log(req.body);
  res.json(req.body);
})

4.1表单post传递参数至后台
  <form method="POST" action="add" name="userform" >
     <input type="text" id="name" name="name" value="xq" class="form-control" />
     <input type="text" id="age" name="age" value="12" class="form-control" />
     <input type="text" id="job" name="job" value="coder" class="form-control" />
     <input type="text" id="hobby" name="hobby" value="run" class="form-control" />
     <button type="submit" class="btn btn-primary">提交添加</button>
  </form>

var express = require('express');
var router = express.Router();

router.route('/add').post(function(req, res){
  var userObj = {};
  userObj = {
    name: req.body.name,
    age: req.body.age,
    job: req.body.job,
    hobby: req.body.hobby
  };
  console.log(userObj);  // {name:'xq',age:'12',job:'coder',hobby:'run'}
});

4.2.jquery ajax传递参数至后台：
　　网站开发当然少不了使用异步传递参数给后台， express4.x中也是以req.body接收异步传递的参数， 完整代码如下：
var _id = '123456';
$.post('/user/delete', {id: _id}, function(data){
    if (data.error){
       $('#removeTips').html('删除异常:' + data.error + '  请刷新重试。');
    }else{
       window.location.href = '/admin/';
    }
 }, 'json’);

var express = require('express');
var router = express.Router();
router.route('/user/delete').post(function(req, res){
    var _id = req.body.id;
    console.log(_id); // 123456
    res.json({result: 'success'});
});

