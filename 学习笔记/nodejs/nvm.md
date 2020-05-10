[TOC]

# 使用nvm对node版本进行控制
## 1. 安装 nvm

安装

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash
```
验证nvm安装完成

```bash
command -v nvm
```
如果安装完成，就会显示如下

nvm
## 2. 查看 nvm 可以安装的 node 版本
查看可以安装的版本
```bash
nvm ls-remote
```
查看所有可以安装的LTS版本（长期支持版）

```bash
nvm ls-remote --lts
```
## 3. 安装指定版本的 node
官方推荐的安装方式 nvm install <版本号>
```bash
$ nvm install v5.5.0
```
但是推荐使用速度更快的方式：使用淘宝源安装

```bash
$  NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install 6
```
默认会安装一个系列中最新的版本

```bash
$ NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install 6     
Downloading and installing node v6.10.2...
Downloading https://npm.taobao.org/mirrors/node/v6.10.2/node-v6.10.2-darwin-x64.tar.gz...
######################################################################## 100.0%
Computing checksum with shasum -a 256
Checksums matched!
Now using node v6.10.2 (npm v3.10.10)
Creating default alias: default -> 6 (-> v6.10.2)
```
也可以在最后指定版本号
```bash
$ NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install 6.10.2
$ NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install 7.9.0
Downloading and installing node v7.9.0...
Downloading https://npm.taobao.org/mirrors/node/v7.9.0/node-v7.9.0-darwin-x64.tar.gz...
######################################################################## 100.0%
Computing checksum with shasum -a 256
Checksums matched!
Now using node v7.9.0 (npm v4.2.0)
```
## 4. 查看已经安装的 node
查看安装的版本 nvm ls
```bash
$ nvm ls
        v6.10.2
->       v7.9.0
default -> 6 (-> v6.10.2)
node -> stable (-> v7.9.0) (default)
stable -> 7.9 (-> v7.9.0) (default)
```
也可以通过目录查看
```bash
/Users/macroot [macroot@macroots-MacBook-Pro] [9:06]
> ls -a ~/.nvm/versions/node
.       ..      v6.10.2 v7.9.0
```
5. 切换 node 版本
切换node版本 nvm use <版本号>

切换至指定版本

$ nvm use v6.10.2
Now using node v6.10.2 (npm v3.10.10)
用node -v确认
```bash
$ node -v 
```
v6.10.2
## 6. 设定默认的 node 版本
设定默认的node版本 nvm alis default <版本号>

$ nvm alias default v6.6.0
default -> v6.6.0
打开新的终端，用nvm current查看当前版本显示
```bash
$ nvm current
```
v6.6.0

## 7. 卸载指定版本的 node
### 7.1 用户权限提升
当使用nvm uninstall <node版本号>的时候，通常会被提示：
```bash
$ nvm uninstall v6.6.0
file is not writable or self-owned: $NVM_DIR/versions/node/v6.6.0/bin/cnpm
Cannot uninstall, incorrect permissions on installation folder.
This is usually caused by running `npm install -g` as root. Run the following commands as root to fix the permissions and then try again.

  chown -R username "$NVM_DIR/versions/node/v6.6.0"
  chmod -R u+w "$NVM_DIR/versions/node/v6.6.0"
```
最后两行的意思是：

第1行：把指定目录的所有者改为 username 所有，这里 username 是用户名，可以改成 $(whoami) 避免输入错误。所以先输入以下命令（使用sudo）：
```bash
$ sudo chown -R $(whoami) "$NVM_DIR/versions/node/v6.6.0"
```
第2行：u+w中u表示所有者，+表示增加权限，w表示可写入。整句表示对目录所有者增加写入权限。所以再输入（使用sudo）：
```bash
$ sudo chmod -R u+w "$NVM_DIR/versions/node/v6.6.0"     
```

### 7.2 删除指定版本 node
当用户有了权限之后，就可以删除指定版本的 node
```bash
$ nvm uninstall v6.6.0
Uninstalled node v6.6.0
```
显示nvm路径
```bash
echo $NVM_DIR
```
如果uninstall无法删除，手动删除
```bash
cd ~/.nvm/versions/node
sudo rm -rf v4.2.3/
```




安装NVM
首先，我们需要得到nvm：
  git clone git://github.com/creationix/nvm.git ~/.nvm
执行成功之后，我们得到了nvm，为了让nvm起作用，我们可以执行：
  . ~/.nvm/nvm.sh
但是，这种做法只能在当前的shell会话中起作用，为了长治久安，一种更好的办法是把它加入到shell的配置文件中，根据不同的环境，它可能是~/.profile，~/.bashrc等等。我们把下面这句加入其中：
  [[ -s "$HOME/.nvm/nvm.sh" ]] && . "$HOME/.nvm/nvm.sh"
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
这样，启动一个新shell会话，我们就可以直接使用nvm了。
好了，环境就绪，我们可以使用它了。

nvm : nodeJS 包管理软件
安装最新稳定版：nvm install stable  
检查版本号：node -v
nvm ls 查看已经安装版本
nvm ls-remote  获取所有版本
安装制定版本   nvm install 0.10.13  
卸载版本 nvm uninstall v4.4.2
安装后需要切换版本nvm use v0.10.13 
nvm use命令只会在当前bash里生效，重新打开一个bash你会发现$PATH的值已经不包含刚才的node目录了，要解决这个问题也很简单，使用nvm alias default <version>命令来指定一个默认的node版本就ok了
查看node安装目录：which node  
使用nvm current 显示当前使用node版本，echo $PATH 查看选的版本


