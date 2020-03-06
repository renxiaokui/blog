---
title: hexo部署和搭建教程
date: 2020-03-06 10:49:24
categories:
    - hexo
    - 搭建教程
tags:
    - hexo
---

# Hexo Github个人博客搭建教程


### github 账号注册

注册github或者gitee账号（注 坑:gitee个人免费版部署需要手动更新）
[github官网](https://github.com/ "github官网")
[gitee官网](https://gitee.com/ "gitee官网")

**注册大家都会吧，嘻嘻**

### nodejs 环境安装

[NodeJs下载](https://nodejs.org/en/ "NodeJs下载")
[Gitr软件下载](https://git-scm.com/ "Gitr软件下载")

安装好以后，随便进入一个目录，右键打开Git Bash Here控制台，输入命令查看是否安装成功
```bash
git version
node -v
npm -v
```
升级npm到最新版 `npm install -g npm`

更新npm源，不然npm下载会很慢
安装nrm `npm install nrm -g --save`
用nrm ls命令查看默认配置，带*号即为当前使用的配置 `nrm ls`
测试nrm速度 `nrm test`
选择最快的源，一般选择taobao `nrm use taobao`

### hexo 环境安装
`npm install -g hexo-cli`

安装 Hexo 完成后，再执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

`hexo init blog`
`cd blog`

如果成功，则恭喜你，失败的话可能是网络，可以尝试 ping下github地址，重新init

`hexo s`

在浏览器中输入 http://localhost:4000 回车就可以预览效果了

### hexo 同步到github
（1） 创建一个新的github仓库
（2） 使用本地git连接远程仓库
```bash
git config --global user.name "你的github用户名"
git config --global user.email "你的github邮箱"
```
使用ssh-keygen生成私钥和公钥:  `ssh-keygen -t rsa`

测试添加ssh是否成功: `ssh -T git@github.com`

如果看到Hi后面是你的用户名，就说明成功了
（3）配置hexo连接到github
找到hexo目录中的_config.yml,拉到末尾修改配置

    deploy:
      type: git
      repo: '你的github或者gitee仓库克隆地址'
      branch: master
安装hexo git插件 `npm install hexo-deployer-git --save`

提交到github上 `hexo d -g`

恭喜，打开你的博客地址就可以访问到了！(gitee同理)

*更多操作请访问：[hexo官网](https://hexo.io/zh-cn/ "hexo官网")*
