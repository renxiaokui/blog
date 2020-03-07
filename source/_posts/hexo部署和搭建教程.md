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

### 本地git连接github或gitee
1. git下载

   [Git官网下载](https://git-scm.com/ "Git下载")

   Git官网下载慢可选择国内镜像安装：[下载](https://npm.taobao.org/mirrors/git-for-windows/ "Git下载")

   安装好以后，随便进入一个目录，右键打开Git Bash Here控制台，输入命令查看是否安装成功 `git --version`

2.  创建github仓库

   进入[github官网](https://github.com/ "github官网")创建一个新的github仓库

3.  生成密钥和github关联

   打开sh Here控制台使用ssh-keygen生成私钥和公钥: `ssh-keygen -t rsa`

   一直点击Enter键就可以了

   进入密钥默认生成目录：`cd ~/.ssh/`

   查看密钥内容并复制：`cat id_rsa.pub`

   进入[github官网](https://github.com/ "github官网")右上角Settings，然后点击SSH and GPG keys，添加一个密钥，粘贴刚刚复制的密钥内容

   测试添加ssh是否成功: `ssh -T git@github.com`

   如果看到Hi后面是你的用户名，就说明成功了

4.  配置git提交用户

   ```bash
   git config --global user.name "你的github用户名"
   git config --global user.email "你的github邮箱"
   ```


### nodejs 环境安装

1. [NodeJs下载](https://nodejs.org/en/ "NodeJs下载")

安装好以后，随便进入一个目录，右键打开Git Bash Here控制台，输入命令查看是否安装成功，如果没有安装git，用cmd也是可以的
```bash
git version
node -v
npm -v
```
2. 更新npm源，不然npm下载会很慢:`npm config set registry https://registry.npm.taobao.org`
检测配置是否成功:`npm config get registry`
安装nrm `npm install nrm -g --save`
用nrm ls命令查看默认配置，带*号即为当前使用的配置 `nrm ls`
测试nrm速度 `nrm test`
选择最快的源，一般选择taobao `nrm use taobao`

注意：如果npm下载报错，可以升级npm到最新版 `npm install -g npm`

### hexo 环境安装
1. 安装hexo插件：`npm install -g hexo-cli`

2. 创建一个博客：

```bash
`hexo init blog`
`cd blog`
```

如果成功，则恭喜你，失败的话可能是npm源配置不好，可以查看npm源配置，或者ping一个github的官网，重新执行init命令

3. 本地启动博客：`hexo s`

在浏览器中输入 http://localhost:4000 回车就可以预览效果了

### hexo 同步到github

找到hexo博客目录中的_config.yml,拉到末尾修改配置

    deploy:
      type: git
      repo: '你的github或者gitee仓库克隆地址'
      branch: master

安装hexo git插件 `npm install hexo-deployer-git --save`

提交到github上 `hexo d -g`

恭喜，打开你的博客地址就可以访问到了！(gitee同理)

*更多操作请访问：[hexo官网](https://hexo.io/zh-cn/ "hexo官网")*
