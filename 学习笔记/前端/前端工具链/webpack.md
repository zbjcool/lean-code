[TOC]

# 总体介绍

静态模块打包工具，
内部构建一个依赖图，生成 bundle

bundle： 新的文件

# 配置项 命令
webpack —config  webpack.config.dev.js(配置文件路径)


通过向 npm run build 命令和你的参数之间添加两个中横线，可以将自定义参数传递给 webpack，例如：npm run build -- —colors。

管理资源：
使用grunt或者gulp处理资源，并将它们从 /src 文件夹移动到 /dist 或 /build 目录中。同样方式也被用于 JavaScript 模块，但是像 webpack 这样的工具，将动态打包(dynamically bundle)所有依赖项（创建所谓的依赖图(dependency graph)）。这是极好的创举，因为现在每个模块都可以明确表述它自身的依赖，我们将避免打包未使用的模块。


## 手动webpack编译
webpack –display-modules –display-chunks –config build/webpack.config.js
在output/static 找到输出信息，输出一个js



# mode模式

区分开发和生产环境
> 通常我们在开发网页时需要区分构建环境

* 开发环境(development) 开发过程中方便开发调试的环境
* 生产环境(production) 发布到线上使用的运行环境

通过npm命令区分
通过cross-env模块设置环境变量
> cross-env 跨平台地设置及使用环境变量,而不必担心为平台正确设置或使用环境变量。


```bash
npm i cross-env -D
```
npm scripts中:
```json
{
    "scripts": {
        "build": "cross-env NODE_ENV=production webpack --mode production",
        "dev": "cross-env NODE_ENV=development webpack-dev-server --mode development",
    }
}
```
执行npm命令切换环境
```bash
npm run build // 生产环境 process.env.NODE_ENV === 'production'
npm run dev // 开发环境 process.env.NODE_ENV === 'development'
```
webpack4以前都是通过DefinePlugin来定义NODE_ENV环境变量，以决定library中应该引用哪些内容。


```js
const webpack = require('webpack');

plugins: [
    new webpack.DefinePlugn({
       'process.env': {
           'NODE_ENV': JSON.stringify(process.env.NODE_ENV)
       }
    })
]
```


在webpack4 增加了mode模式配置 在浏览器环境下指定了 process.env.NODE_ENV 的值，默认是development，但node环境中还是需要cross-env来设置。
mode 是 webpack 4 中新增加的参数选项，其有两个可选值：production 和 development。mode 不可缺省，需要二选一：

mode：过选择 development, production 或 none 之中的一个，来设置 mode 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 production。
# entry，output

entry： todo:多文件如何配置
output：{
    path，
    fileName
}
新生成 文件放哪里
    // 入口文件，path.resolve()方法，可以结合我们给定的两个参数最后生成绝对路径，最终指向的就是我们的index.js文件
    entry: path.resolve(__dirname, '../app/index/index.js'),

# module.loader

module指各种资源文件，如js、css、图片、svg、scss、less等等，一切资源皆被当做模块。


loader处理各种资源
不同资源用不同loader


module => rules 
webpack 根据正则表达式，来确定应该查找哪些文件，并将其提供给指定的 loader。
如：test:/\.css$/ 


为了从 JavaScript 模块中 import 一个 CSS 文件，你需要在 module 配置中 安装并添加 style-loader 和 css-loader：

test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
use 属性，表示进行转换时，应该使用哪个 loader。
webpack自身只支持js和json这两种格式的文件，它是一个转换器，将A文件进行编译成B文件，比如：将A.less转换为A.css，单纯的文件转换过程。
css-loader、style-loader、postcss-loader、sass-loader


webpack的哲学是一切皆是模块，无论是js/css/sass/img/coffeejs/ttf….等等，因此, Webpack 当中 js 可以引用 css, css 中可以嵌入图片 dataUrl
2.webpack可以使用自定义的loader去把一切资源当做模块加载，这样就解决了模块依赖的问题，对应各种不同文件类型的资源, Webpack 有对应的模块 loader。
同时，利用插件还可以对项目进行优化，由于模块的加载和项目的构建优化都是通过webpack一个”人“来解决的，所以模块的加载和项目的构建优化并不是无机分离的，
而是有机的结合在一起的，是一个组合的过程，这使得webpack在这方面能够完成的更出色，这也是webpack的优势所在。



## 优化babel-loader
[babel-loader](https://webpack.docschina.org/loaders/babel-loader/)
cacheDirectory 缓存在node_module/.cache

要排除 node_modules


# pugings
打包，优化，压缩，重新定义环境变量
想要使用插件，require()它，添加到plugins数组中，

plugin是一个扩展器，它丰富了webpack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务
## babel-polyfill
## html-webpack-plugin

这个插件可以创建html文件，并自动将依赖写入html文件中。
## clean-webpack-plugin
## html-webpack-template

[html-webpack-template](https://github.com/jaketrent/html-webpack-template/blob/86f285d5c790a6c15263f5cc50fd666d51f974fd/index.html)


### uglifyjs-webpack-plugin uglify-webpack-plugin
webpack 生产环境打包去掉console.log
1. 安装
```
$ npm install uglifyjs-webpack-plugin --save-dev
```
2. 配置webpack.prod.config.js
```js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

module.exports = {
  //...
  optimization: {
      minimizer: [new UglifyJsPlugin({
        uglifyOptions: {
          compress: {
            drop_console: true,
          }
        }
      })]
  }
};
```



# 自动编译
webpack 中有几个不同的选项，可以帮助你在代码发生变化后自动编译代码：

1. webpack's Watch Mode  
2. webpack-dev-server
3. webpack-dev-middleware



webpack's Watch Mode 
webpack —config xxxx.js --watch
唯一的缺点是，为了看到修改后的实际效果，你需要刷新浏览器。如果能够自动刷新浏览器就更好了，
webpack-dev-server 为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。让我们设置以下：

webpack-dev-middleware：

1、不会向硬盘写文件，而是在内存中，注意我们构建项目实际就是向硬盘写文件。
2、当文件改变的时候，这个中间件不会再服务旧的包，你可以直接帅新浏览器就能看到最新的效果，这样你就不必等待构建的时间，所见即所得。

## webpack-dev-middleware

要搭配express使用
Now we need to make some adjustments to our webpack configuration file in order to make sure the middleware will function correctly:
就是还需要增加publicPath:’/'

### code splitting

### 参考





# 相关问题

## webpack4 升级
[webpack4 升级或使用的几点采坑记录](https://github.com/iuap-design/blog/issues/262)

## -webpack-hmr 404 not found   
加端口号问题解决

## 引入公共库（jquery）
[simplify the life](https://www.cnblogs.com/zichi/p/7787669.html)
[如何在 webpack 中引入未模块化的库，如 Zepto](https://sebastianblade.com/how-to-import-unmodular-library-like-zepto/)

## 分析webpack
[统计数据](https://webpack.docschina.org/api/stats/)

生成分析文件：webpack --profile --json > stats.json
如果报错： webpack --profile Entry module not found: Error: Can't resolve './src' in 'xxxx'
webpack 默认入口文件是src/index.js 修改入口文件名称
[stackoverflow](https://stackoverflow.com/questions/45255190/entry-module-not-found-error-cant-resolve-src-index-js)

# 参考

[参考](https://github.com/dongyuanxin/webpack-demos)


[soucemap](https://blog.csdn.net/liwusen/article/details/79414508)

[官网](https://webpack.js.org/configuration/devtool/#development)
[webpack4.0打包优化](https://juejin.im/post/5ac769e7f265da237b225490)

[webpack 4 Code Splitting 的 splitChunks 配置探索](http://imweb.io/topic/5b66dd601402769b60847149)
