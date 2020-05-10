s
[TOC]
## 起步
### 版本控制
**关于版本控制**
> 版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。
* 将版本或者项目回溯到某个时间点
* 比较文件的变化，找出产生bug的原因
**本地版本控制系统**
![77d33f3018af1a42487fc96a554950e7.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p46)

**集中化的版本控制系统**
有一个集中管理的服务器，保存所有文件的修订版本。协同工作的人们通过客户端连接到服务器，取出文件或者提交更新
![4838fd4dd71808f1ae1fb65c07a4896e.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p47)
缺点：
* 中央服务器宕机，其他人都不能获取文件和提交更新
* 如果中心数据库所在的磁盘发生损坏，又没有做恰当备份，毫无疑问你将丢失所有数据——包括项目的整个变更历史，只剩下人们在各自机器上保留的单独快照。 本地版本控制系统也存在类似问题，只要整个项目的历史记录被保存在单一位置，就有丢失所有历史更新记录的风险。
**分布式版本控制系统**
> 客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。
![289ee13ff077319387caa5cb396c6cea.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p48)


### git简介
> git是分布式版本管理协议。git只能知道文本文件的具体改动，二进制文件无法知道具体改动，比如图片，word。
> 现在有两个版本v1和[**V2**](https://git-scm.com/book/zh/v2)

本地创建一个目录，目录里面有些文件。
cd 到该目录（工作区），git init后会创建.git目录，.git就是repository（仓库）,仓库里面有暂存区（stage或者index），git add就是添加修改到暂存区，git commit就是提交暂存区的修改文件到本地仓库的HEAD指向的当前分支，git push就是提交修改到远程仓库

提交工作区修改到暂存区，git add
撤销工作区修改，git checkout -- file
一种是自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

提交暂存区的文件修改到本地仓库，git commit
撤销暂存区修改，git reset HEAD <file>

本地仓库提交到远程仓库： git push
本地仓库版本回退： git log 查看版本提交情况，git reflog 查看git命令历史，使用git reset --hard HEAD commitid或者^ ^^ ~100

后面可以创建一个远程仓库，与本地仓库关联
可以从这个仓库克隆出新的仓库（git clone），也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
$ git remote add origin git@github.com:michaelliao/learngit.git



### git config
#### 配置文件存储：
1. 系统配置文件存储在/etc/gitconfig
git config --system -l
2. 当前用户的配置文件在~/.gitconfig 或 ~/.config/git/config 文件
使用git config --global -l查看
3. 当前使用仓库的 Git 目录中的 config 文件（就是 .git/config）：针对该仓库。
ps: 每一个级别覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。

#### 用户信息

* 使用如下命令显示所有配置下
git config --list
可以使用git config key：
列出某一个配置项
* 使用下面命令配置name和email
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
--global 可以替换成 --system,不填是当前项目
注意：
* 如果使用了--global,本系统的所有项目都会使用global的信息
* 如果不想使用global的信息
    1. 当前项目.git/config配置文件中加入
        [user]
        name = zbj
        email =                     binjue.zheng@sqmall.com
        当前项目的用户和email就改了，覆盖了上一级的config。
        
    2. 使用$ git config user.name "John Doe"
$ git config user.email johndoe@example.com
### 帮助
$ git help verb
$ git verb --help
$ man git-verb

## git基础
### git add
[参考](https://blog.csdn.net/liuyanggofurther/article/details/8460160)
1. git add pathSpec 
a leading directory dir dir/file1 dir/file2
 Note that older versions of Git
used to ignore removed files;
2. git add *.c fileglobs（文件通配符） 
globs（团） e.g. *.html
3. git add . current dir created, modified,remove file
4. git add --all 工作树中所有新增，修改，删除的文件提交到index
5. git add -u 新增的文件不提交
6. git add -v 显示详细信息
7. git add -p 交互式的提交
8. git add -e 打开编辑器提交
9. git add -f 把ignore的文件也添加进入
10. git add -i 交互式的修改文件状态


### git branch

1. 本地分支重命名(还没有推送到远程)
```bash
git branch -m oldName newName
```
2. 远程分支重命名 (已经推送远程-假设本地分支和远程对应分支名称相同)
1. 重命名远程分支对应的本地分支
```bash
git branch -m oldName newName
```
2. 删除远程分支
使用下面两条命令来删除远程分支

git branch -r -d origin/branch-name
git push origin :branch-name

update:

解释一下上面的参数含义：

       -r, --remotes
           List or delete (if used with -d) the remote-tracking branches.
上面的第一句是删除了本地的远程跟踪分支（ 我也不知道怎么描述更加清楚），此时使用git branch -a查看，分支remotes/origin/branch-name应该已经不存在了。
```bash
git push origin :oldName
// 后面可以多加几个，删除多个
git branch -r -d origin/name
```
3. 上传新命名的本地分支
```bash
git push origin newName
```
4.把修改后的本地分支与远程分支关联
```
git branch --set-upstream-to origin/newName
```
3. git remote prune origin

假设这样一种情况：

我创建了本地分支b1并pull到远程分支 origin/b1；
其他人在本地使用fetch或pull创建了本地的b1分支；
我删除了 origin/b1 远程分支；
其他人再次执行fetch或者pull并不会删除这个他们本地的 b1 分支，运行 git branch -a 也不能看出这个branch被删除了，如何处理？
使用下面的代码查看b1的状态：
```bash
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:xxx/xxx.git
  Push  URL: git@github.com:xxx/xxx.git
  HEAD branch: master
  Remote branches:
    master                 tracked
    refs/remotes/origin/b1 stale (use 'git remote prune' to remove)
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)

```
这时候能够看到b1是stale的，使用 git remote prune origin 可以将其从本地版本库中去除。

更简单的方法是使用这个命令，它在fetch之后删除掉没有与远程分支对应的本地分支：
```bash
git fetch -p
```

### git log

git log -g
显示 reflog
### git reset

如果git merge 一个分支
不想合并改分支，可以用git reset --hard HEAD
```bash
git reset --hard HEAD^ //用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^,往上100个版本写成HEAD~100

git reset --hard 版本号 //版本号没必要写全，前几位就可以了，Git会自动去找

git reset -- filename // unstage file，把文件从暂存区拉回到工作区
```

### git reflog
数据恢复
Git记录每次修改HEAD的操作，
git reflog/git log -g可以查看所有的历史操作记录，然后通过git reset命令进行恢复。


### git diff
查看工作区和版本库里面最新版本的区别

```bash
git diff // diff 所有

git diff -- filename // diff 该文件
```


### git checkout
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
```bash
git checkout -- filename // 丢弃工作区的修改
git checkout branchname // 如果没有--是切换分支

git checkout -b branchname //新建分支
```

### git stash
```bash
git stash list
1. 是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
2. git stash pop，恢复的同时把stash内容也删了：
```



git clone 整个仓库后使用，以下命令就可以取得该 tag 对应的代码了。 

git checkout tag_name 
但是，这时候 git 可能会提示你当前处于一个“detached HEAD" 状态。

因为 tag 相当于是一个快照，是不能更改它的代码的。

如果要在 tag 代码的基础上做修改，你需要一个分支： 

git checkout -b branch_name tag_name
这样会从 tag 创建一个分支，然后就和普通的 git 操作一样了。


如果项目上有一个后来新建的分支test，并且使用

git branch -a
看不到该远程分支：

* develop
  remotes/composer/develop
  remotes/composer/feature/194
  remotes/composer/feature/198
  remotes/composer/feature/199
  remotes/composer/feature/200
  remotes/composer/master
  remotes/origin/HEAD -> origin/develop
  remotes/origin/develop
  remotes/origin/feature/194
  remotes/origin/feature/198
  remotes/origin/feature/199
  remotes/origin/feature/200
  remotes/origin/master
直接使用命令git checkout test，出现以下错误

error: pathspec 'origin/XXX' did not match any file(s) known to git.
项目上有一个分支test，使用git branch -a看不到该远程分支，直接使用命令git checkout test报错如下：
解决方法是：

1、执行命令git fetch取回所有分支的更新

2、执行git branch -a可以看到test分支（已经更新分支信息）

3、切换分支git checkout test




git简介git是分布式版本管理协议。git只能知道文本文件的具体改动，二进制文件无法知道具体改动，比如图片，word。配置git&nbsp;$ git config --global user.name "Your Name"&nbsp;$ git config --global user.email "email@example.com"创建git使用git init命令。添加文件到Git仓库，分两步：第一步，使用命令git add &lt;file&gt;，注意，可反复多次使用，添加多个文件；第二步，使用命令git commit -m ""，完成。-m后面是本次提交说明。git status 查看仓库当前的状态，git diff &lt;file&gt; 查看该文件具体改动版本回退：HEAD指向的版本就是当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写成HEAD~100。因此，Git允许我们在版本的历史之间穿梭，使用命令$ git reset --hard HEAD^    或者$ git reset --hard commit_id。穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。工作区和暂存区：Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。工作区（Working Directory）就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区，除了.git文件版本库（Repository）工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。![24b6ae4f24eb79047f1350722ffb726e.png](evernotecid://0C31C67B-DB73-40A3-9A1A-E7CB36E4C703/appyinxiangcom/2718176/ENResource/p120)
我们把文件往Git版本库里添加的时候，是分两步执行的：第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区（stage）；git add 文件名git add . 递归检查当前目录下的所有文件修改情况，把修改过的加入index中第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。管理修改：Git跟踪并管理的是修改，而非文件。每次修改，如果不add到暂存区，那就不会加入到commit中。撤销修改：场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。修改未暂存区域文件场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。关联一个远程库要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；关联后，使用命令git push -u origin master第一次推送master分支的所有内容；此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！从远程库克隆到本地要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。git branch &lt;branchname&gt; 创建新的分支git branch -D &lt;branchname&gt; 删除名称为branch的本地分支-d, —delete&nbsp;delete fully merged branch; -D&nbsp;delete branch (even if not merged)git branch -r -d &lt;branchname&gt; 删除一个tracking的远端branchgit push origin :branchName 删除远程分支，原理是把一个空分支push到server上git push origin :phaser-feature-20170822-testgit checkout -b branchname &nbsp;创建一个新的分支，并且切换到创建的新的分支git branch -a &nbsp;查看本地和远程分支git branch -r 查看远程分支git branch 查看本地分支git checkout &nbsp;branchName 切换分支remotes/origin/HEAD -&gt; origin/masterorigin &nbsp;相当于一个别名，远程git地址的一个别名，远程git地址用git remote -v查看13.git stashgit stash listgit stash apply stash@{n} &nbsp;apply 不删除该stashgit stash drop stash@{n} 删除该stashgit&nbsp;stash pop stash@{0} pop 后删除追踪分支&nbsp;在Git中‘追踪分支’是用与联系本地分支和远程分支的. 如果你在’追踪分支'(Tracking Branches)上执行推送(push)或拉取(pull)时,　它会自动推送(push)或拉取(pull)到关联的远程分支上.如果你经常要从远程仓库里拉取(pull)分支到本地,并且不想很麻烦的使用"git pull "这种格式; 那么就应当使用‘追踪分支'(Tracking Branches).‘git clone‘命令会自动在本地建立一个'master'分支，它是'origin/master'的‘追踪分支’. 而'origin/master'就是被克隆(clone)仓库的'master'分支.译者注: origin一般是指原始仓库地址的别名.你可以在使用'git branch'命令时加上'--track'参数, 来手动创建一个'追踪分支'.git branch&nbsp;--track&nbsp;experimental&nbsp;origin/experimental当你运行下命令时:$&nbsp;git pull&nbsp;experimental它会自动从‘origin'抓取(fetch)内容，再把远程的'origin/experimental'分支合并进(merge)本地的'experimental'分支.当要把修改推送(push)到origin时, 它会将你本地的'experimental'分支中的修改推送到origin的‘experimental'分支里,　而无需指定它(origin).git commit 撤销git log我们常用&nbsp;-p&nbsp;选项展开显示每次提交的内容差异，用&nbsp;-2&nbsp;则仅显示最近的两次更新：有许多摘要选项可以用，比如&nbsp;--stat，仅显示简要的增改行数统计：git reset —hard commit-id分支重命名本地分支重命名git branch -m old new远程分支重命名删除远程分支git push origin :远程分支名(你要删除的远程分支名)将本地分支推送到远程分支上，如果远程分支不存在，则创建此远程分支git push origin 本地分支名:远程分支名git subtree &nbsp;git remotegit remote &nbsp;-v 列出远程仓库git remote add ios-dev-kit git@gitlab.yuanpin-inc.com:ios/ios-dev-kit.git &nbsp;添加远程仓库git subtree push --prefix ios-dev-kit/ ios-dev-kit mastergit add .&nbsp;.只能够提交当前目录或者它后代目录下相应文件。