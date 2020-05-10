[TOC]

## vscode设置缩进2个空格

首选项=>设置=>"editor.tabSize": 2


## vscode 配置 eslint 自动格式化
找下，应该也有
1. 全局安装
npm install -g eslint 
eslint --init
//按照操作一步一步选
2. vscode搜索 eslint 插件，并安装
3. 配置
com + shift + p 
搜索 setting ，加入下面配置
"eslint.format.enable": true, 
//autoFix默认开启，只需输入字符串数组即可 
"eslint.validate": ["javascript", "vue", "html"] 

完成后如果格式有问题，保存代码自动修改成符合格式的代码，rule 也可以自行配置