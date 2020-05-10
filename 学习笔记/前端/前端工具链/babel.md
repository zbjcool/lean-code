
# 介绍
Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。


// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
  return item + 1;
});
一、配置文件.babelrc

Babel的配置文件是.babelrc，存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。
该文件用来设置转码规则和插件，基本格式如下。

{
  "presets": [],
  "plugins": []
}
presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

## ES2015转码规则
$ npm install --save-dev babel-preset-es2015

## ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
然后，将这些规则加入.babelrc。

  {
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }


## babel-polyfill

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。


# 原理
babel 背后原理很多地方用到

解析: 将代码(其实就是字符串)转换成 AST( 抽象语法树) 使用 babylon
转换: 访问 AST 的节点进行变换操作生成新的 AST
生成: 以新的 AST 为基础生成代码
1. 代码解析
    1.1 代码解析,也就是我们常说的 Parser, 用于将一段代码(文本)解析成一个数据结构.
    1.2 词法分析(Tokenizer -- 词法分析器)
    1.3 语法分析
2. 代码转换
    2.1 遍历抽象语法树(实现遍历器traverser)
    2.2 转换代码(实现转换器transformer)
3. plugins
继续过滤，增加删掉代码
4. 把 AST 变成code生成代码(实现生成器generator)


# 工具
@babel/core
1. babel-loader: 负责 es6 语法转化
2. babel-preset-env: 包含 es6、7 等版本的语法转化规则,.babelrc中 presets
3. babel-polyfill: es6 内置方法和函数转化垫片
4. babel-plugin-transform-runtime: 避免 4. polyfill 污染全局变量