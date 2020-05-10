
[TOC]

# html结构
HTML 是一种描述 Web 文档结构和语义的语言；它由元素组成，每个元素可以有一些属性。
* 根元素 <html> 元素 这个元素包含了整个页面的内容，有时也被称作根元素。
* 文档类型：<!DOCTYPE html> 不是标签，是声明，以前用来链接一些 HTML 编写守则，现在不用管，加上用就好，历史遗留
```html
<!DOCTYPE html>
<html>
<head></head>
<body></body>
</html>
```

# 元素
* 元素定义
HTML 由一系列的元素（elements）组成，元素是指用标签包围不同部分的内容，HTML 标签里的元素名不区分大小写
![4bb74632b6bb2f92ba73435d816a6fe8.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p122)

* 元素的属性和属性的值
![0a6de6724fdfe8527bfe2aa8e43bab4e.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p123)

* 空元素
一个不可能存在子节点（例如内嵌的元素或者元素内的文本）的element。
比如img标签，没有</img>，也没有内容
```html
<img src="images/firefox-icon.png" alt="测试图片">
```

## 主根元素
<html>	HTML <html> 元素 表示一个HTML文档的根（顶级元素），所以它也被称为根元素。所有其他元素必须是此元素的后代。

## 元数据
描述文档的一些基本信息
### head
这个元素放置的内容不是展现给用户的，而是包含例如面向搜索引擎的搜索关键字（keywords）、页面描述、CSS 样式表和字符编码声明等。
### base
<base> HTML <base> 元素 指定用于一个文档中包含的所有相对 URL 的根 URL。一份中只能有一个 <base> 元素。
### title
这个元素设置页面的标题，显示在浏览器标签页上，同时作为收藏网页的描述文字。
### link
* <link rel="dns-prefetch" href="//cbu01.alicdn.com”>
dns加速
* <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"> 添加图标
* <link rel="stylesheet" href="my-css-file.css">
stylesheet 指链接样式，这个通常放到头部
### style
里面直接是 css
#### script
<script> 部分没必要非要放在文档头部；实际上，把它放在文档的尾部（在 </body>标签之前）是一个更好的选择，这样可以确保在加载脚本之前浏览器已经解析了HTML内容（如果脚本加载某个不存在的元素，浏览器会报错）。
您还可以选择将脚本放入<script>元素中，而不是指向外部脚本文件。
### meta
meta 标签用来添加元数据，元数据就是描述数据的数据
* <meta charset="utf-8"> — 这个元素指定了当前文档使用 UTF-8 字符编码 ，UTF-8 包括绝大多数人类已知语言的字符。基本上 UTF-8 可以处理任何文本内容，还可以避免以后出现某些问题，我们没有任何理由再选用其他编码。

* 作者和描述
许多<meta> 元素包含了name 和 content 特性：
name 指定了meta 元素的类型； 说明该元素包含了什么类型的信息。
content 指定了实际的元数据内容。
添加作者和描述，为了 seo，搜索引擎可能显示 description
```html
<meta name="author" content="Chris Mills">
<meta name="description" content="xxx">
```
 但是 keywork已经被搜索引擎忽略了，因为作弊者填充了大量关键词到keyword， 错误地引导搜索结果。
 
* 视觉视口：visual viewpost：用户通过屏幕真实看到的区域。
我们可以借助<meta>元素的viewport来帮助我们设置视口、缩放等，从而让移动端得到更好的展示效果。
```html
<meta name="viewport" content="width=device-width; initial-scale=1; maximum-scale=1; minimum-scale=1; user-scalable=no;">
```
* <meta http-equiv="X-UA-Compatible" content="IE=Edge">
#以上代码IE=edge告诉IE使用最新的引擎渲染网页
* <meta name="renderer" content="webkit”>
双核浏览器采用webkit

## 分区根元素
<body>	HTML body 元素表示文档的内容。document.body 属性提供了可以轻松访问文档的 body 元素的脚本。

## 语义化
- 正确的标签做正确的事情
- 页面内容结构化
- 无CSS样子时也容易阅读，便于阅读维护和理解
- 便于浏览器、搜索引擎解析。 利于爬虫标记、利于SEO
总之，HTML语义化是反对大篇幅使用无语义化的div+span+class，而鼓励使用HTML定义好的语义化标签，方便搜索引擎识别
语义化标签
```html
<header>
    <nav></nav>
</header>
<main>
<h1></h1>
<h2></h2>
</main>
<aside>
    <section>
        <header></header>
        <div></div>
    </section>
<aside>
<article></article>
<footer></footer>
```
* header 页头
* footer 页尾
* article 内容
* aside 旁边目录，菜单
* main
* nav
* section
* h1-h6
## 块元素
块元素内嵌在块元素里面通常会以新行开始
* div
* dd dl dt
* p
* ul ol li
## 行内元素
也叫内联元素 ，一个行内元素只占据它对应标签的边框所包含的空间
内联元素内嵌在块元素里面通常不会以新行开始，和块元素的内容在一行内显示
* <a> — a 是 "anchor" 
href 指hypertext reference
```html
<a href="https://www.mozilla.org/zh-CN/about/manifesto/">Mozilla 宣言</a>
```
* <br/>
* span

## 替换型标签
替换型元素是把文件的内容引入，替换掉自身位置的一类标签。有 src 属性，link 标签不是 src，是 href 所以不是替换型标签
### 图片和多媒体
音视频元素，video，audio的增加使得我们不需要在依赖外部的插件就可以往网页中加入音视频元素。
* <img>
* <audio>
* <video>

### 内嵌内容
* <iframe>

### script src

## 表单
HTML 提供了许多可一起使用的元素，这些元素能用来创建一个用户可以填写并提交到网站或应用程序的表单。详情请参阅 HTML 表单指南。

* <button>	HTML <button> 元素表示一个可点击的按钮，可以用在表单或文档其它需要使用简单标准按钮的地方。
* <datalist>	HTML <datalist>元素包含了一组<option>元素，这些元素表示其它表单控件可选值.	
* <form>	HTML <form> 元素表示了文档中的一个区域，此区域包含有交互控制元件，用来向 Web 服务器提交信息。
* <input>	HTML <input> 元素用于为基于Web的表单创建交互式控件，以便接受来自用户的数据; 可以使用各种类型的输入数据和控件小部件，具体取决于设备和user agent。
* <label>	HTML <label> 元素（标签）表示用户界面中某个元素的说明。
<select>	HTML <select> 元素表示一个控件，提供一个选项菜单：
<textarea>	HTML <textarea> 元素表示一个多行纯文本编辑控件。


# 语言
## 实体 entity
html 使用实体进行转义

```
space &nbsp;
&	&amp;
<	&lt;
>	&gt;
"	&quot;
```
## 命名空间

xmlns 属性



# 嵌入js脚本
1. <script></script>内嵌
2. script标签src
3. html 中 类似onclick事件
4. 放在 url,使用"javascript:"协议



