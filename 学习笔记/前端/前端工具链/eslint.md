[toc]
# 介绍


## 原理
对代码的检查基于规则

## ESlint “window” is not defined, “document” is not defined
以下三种方法选择任意一种都可以解决。个人推荐第二种。

## 检查等级
1. off 或者0
关闭规则
2. warn 或者1
开启规则，警告级别，不会退出程序
3. error或者2
开启规则，错误级别，会退出程序


## 插件
eslint-plugin-standard
eslint-plugin-vue
eslint-plugin-prettier
eslint-plugin-node

## 自定义插件
自己定义规则




1. 添加到 .eslintrc.js

module.exports = {
  "globals": {
    "window": true,
    "document": true,
  },
  ...
}
2. 或者添加到 .eslintrc.js

module.exports = {
  "env": {
    "browser": true,
    "node": true,
  },
  ...
}
3. 或者添加到 package.json:

"eslintConfig": {
  "globals": {
    "window": true
  }
}


