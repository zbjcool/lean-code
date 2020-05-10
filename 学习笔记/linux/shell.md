## shell
打开一个terminal，会运行shell，常用bash。
1. shell是一个应用程序，是壳，内核外面的壳，外面是用户，用户通过shell和操作系统内核交互，输入命令，执行、反馈。
2. shell是一种脚本语言，在windows叫bat，即批处理程序，可以定义变量，有循环、流程控制，执行命令。

1. 在一般的linux系统当中（如redhat），使用sh调用执行脚本相当于打开了bash的POSIX标准模式
2. 也就是说 /bin/sh 相当于 /bin/bash —posix
所以，sh跟bash的区别，实际上就是bash有没有开启posix模式的区别

3. 第一个脚本
hello.sh
```bash
#!/bin/sh
echo "hell shell"
```
#!/bin/sh 
表示本脚本由/bin/路径的sh程序来解释。

4. 查看历史使用
第一种方法
history | less
比如 1  zsh --version
    2  echo $SHELL
    3  /bin/zsh
    4  ls
    5  cd Documents
    6  cd ..
    7  ls
找到行号
输入!7
第二种方法
ctr+r
ctr+p 上一个
ctr+n 下一个
!!:上一次输入的命令

1. 作为可执行程序
./hello 运行
需要修改文件权限
chmod +x ./hello

2. 作为解释器参数
sh ./hello.sh
不需要可执行权限

## shell脚本语法

1. #代表注释
2. #!字符序列是一种特殊的结构叫做 shebang
3. 让我们的脚本可执行 chmod a+x xxx.sh
4. 脚本不需要加后缀sh
5. 这个点（.）命令是 source 命令的同义词，执行脚本
6. 所有用户的脚本在/usr/local/bin，当前用户在$HOME，修改zshrc或者bashrc增加PATH=$HOME/bin:$PATH
7.长短命令是等价的命令。为了减少输入，当在命令行中输入选项的时候，短选项更受欢迎，但是当书写脚本的时候， 长选项能提供可读性。

变量
1. 不需要预先定义：当 shell 碰到一个变量的时候，它会 自动地创建它
2. shell 不能辨别变量和常量，用大写代表常量，小写变量
赋值： 
a=z                     # Assign the string "z" to variable a.
b="a string"            # Embedded spaces must be within quotes.
c="a string and $b"     # Other expansions such as variables can be
                        # expanded into the assignment.

d=$(ls -l foo.txt)      # Results of a command.
e=$((5 * 7))            # Arithmetic expansion.
f="\t\ta string\n"      # Escape sequences such as tabs and newlines.

使用： $xxx
变量名可能被花括号 “{}” 包围着。


