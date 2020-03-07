---
title: Git配置同时连接多个远程地址
date: 2020-03-07 15:22:29
categories:
    - git
tags:
    - git
---

### Git配置同时连接多个远程地址

*注:此教程是同时绑定gitee和github*

1. 右键使用Git Bash Here 进入ssh目录

   `cd ~/.ssh`
   注:如果没有此目录,可能你第一次安装,可以执行ssh-keygen会自动生成一个key和目录

2. 创建配置文件

   ```
   touch config
   vi config
   ```

   ```
   # gitee
   Host gitee.com
   HostName gitee.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/gitee_id_rsa
   
   # github
   Host github.com
   HostName github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/github_id_rsa
   ```

3. 创建sshkey

   ```
   ssh-keygen -t rsa -C "xxx@qq.com" -f "github_id_rsa"
   ssh-keygen -t rsa -C "xxx@qq.com" -f "github_id_rsa"
   ```

4. 配置sshkey

   查看密钥内容并复制：`cat github_id_rsa`

    进入[github官网](https://github.com/ "github官网")右上角Settings，然后点击SSH and GPG keys，添加一个密钥，粘贴刚刚复制的密钥内容，Gitee同理

5. 测试是否成功

   ```
   ssh -T git@gitee.com
   ssh -T git@github.com
   ```

   如果看到Hi后面是你的用户名，就说明成功了

