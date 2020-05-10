npm

npm init

npm 帮你完成package.json 文件

mkdir blog && cd blog
npm init
npm init —yes或者-y 就会使用一些默认值进行初始化。
npm install xxxx --save

npm install async --save
上面以安装 async 为例，后面跟了一个参数 --save 正是这个参数的功能，帮我们下载依赖包后直接将依赖写入 package.json 文件中了，版本再也不会搞错，非常贴心。

npm install moduleNames //本地安装node模块
npm install -g moduleNames //全局安装node模块

node的安转分为全局模式和本地模式
本地模式会安装到你的应用程序目录下的本地node_modules下
全局模式下，会被安装到Node的安装目录下的node_modules

但是代码中，直接通过require()的方式是没有办法调用全局安装的包的。全局的安装是供命令行使用的，就好像全局安装了vmarket后，就可以在命令行中直接运行vm命令

npm install —save-dev moduleNames //devDependencies
npm install —save moduleNames //dependencies

如果远程仓库有了新版本也不重新安装
npm run start

npm 帮我们启动项目，如何完成，需要在 package.json 文件中制定启动脚本，代码如下

"scripts": {
  "start": "node app.js"
}
这个功能对于项目开发者可能不是特别重要，但是对于使用此项目的人非常方便。

比如你的项目开源后，好多开发者克隆到本地，安装依赖包后该如何启动，你的根目录下有index.js,start.js,app.js,my.js 鬼才知道哪个是启动文件，所以使用你项目的人必须一个一个打开看看代码，到底是哪个文件启动了node 服务器，而如果你在 package.json 中指定了启动脚本，用npm 来启动项目非常方便，如下：

npm start



npm outdated
检查包是否已经过时，此命令会列出所有已经过时的包，可以及时进行包的更新

11、npm update moduleName：更新node模块，会安装远程仓库中新的版本

12、npm uninstall —save moudleName：卸载node模块

可以使用npm-check
全局安装npm-check： npm install -g npm-check
npm-check -u
上下移动，空格选择
缓存目录:

npm install或npm update命令，从 registry 下载压缩包之后，都存放在本地的缓存目录。
这个缓存目录，在 Linux 或 Mac 默认是用户主目录下的.npm目录，
通过配置命令，可以查看这个目录的具体位置。


$ npm config get cache
$HOME/.npm
每个模块的每个版本，都有一个自己的子目录，里面是代码的压缩包package.tgz文件，以及一个描述文件package/package.json。

/Users/zhengbj/.nvm/versions/node/v9.1.0
'npm -g'安装的node模块的命令都在.nvm/versions/node/v9.1.0/bin目录下
代码在.nvm/versions/node/v9.1.0/lib/node_modules
模块安装过程：
1. 发出npm install命令
2. npm 向 registry 查询模块压缩包的网址
3. 下载压缩包，存放在~/.npm目录
4. 解压压缩包到当前项目的node_modules目录


npm search moduleName

npm outdated

