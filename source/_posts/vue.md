---
title: vue
date: 2020-06-05 21:33:04
tags:
---

# 前端知识

## nodejs安装 

## http://nodejs.cn/download/

安装cnpm

`npm install -g cnpm --registry=https://registry.npm.taobao.org`

ps：安装yarn

`npm install -g yarn`

切换为淘宝镜像源：npm config set registry http://registry.npm.taobao.org/

或者使用nrm管理npm镜像源

```
npm install nrm -g --save
nrm ls
nrm use taobao
```



## webpack安装

`cnpm install -g webpack`

从webpack 4.X开始，需要全局安装webpack-cli，写如下命令

`cnpm install webpack webpack-cli -g`



## vue安装 

`cnpm install vue-cli -g`

指定版本安装:

​		如是3.0以下 `npm install -g vue-cli@版本号`

​		如是3.0以上 `npm install -g @vue/cli@版本号`

3.0以上创建项目：

方式一：`vue create 项目名称`

方式二：`vue ui`

启动项目：npm run dev



## 更新版本

升级node：

```undefined
npm install -g n 遇到错误加上–force
n list
n stable
```

npm 更新命令：`npm install -g npm`

vue-cli脚手架更新命令：`npm install -g @vue/cli`

vue卸载：npm uninstall vue-cli -g



## 请求封装axios

```
npm install axios;
```

*//main.js*

import axios from "axios";