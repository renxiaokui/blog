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
   注:如果没有此目录,可能你第一次安装,可以执行`ssh-keygen`会自动生成一个key和目录

2. 创建sshkey

   ```
   ssh-keygen -t rsa -C "xxx@qq.com" -f "github_id_rsa"
   ssh-keygen -t rsa -C "xxx@qq.com" -f "gitee_id_rsa"
   ```

3. 创建配置文件

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

4. 配置sshkey

   查看密钥内容并复制：`cat github_id_rsa`

    进入[github官网](https://github.com/ "github官网")右上角Settings，然后点击SSH and GPG keys，添加一个密钥，粘贴刚刚复制的密钥内容，Gitee同理

5. 测试是否成功

   ```
   ssh -T git@gitee.com
   ssh -T git@github.com
   ```

   如果看到Hi后面是你的用户名，就说明成功了

### 同时上传一个项目到两个地址

克隆项目到本地：`git clone 'https://gitee地址'`

查看项目远端情况：`git remote -v`

#### 方式一：`git remote set-url`命令

*注意：项目的分支名必须是一样的，不一样会自动创一个新的分支*

* 添加其他远端仓库地址：`git remote set-url --add origin 'https://github地址或者其他仓库'`

* 推送到远程仓库：`git push`

* 如果有冲突说明两个仓库代码不一致，可使用命令强制覆盖：`git push -f`


#### 方式二：`git remote add` 命令

添加其他远程仓库地址：`git remote add gitee(远端名称自定义) https://地址`

查看项目远端情况，可以看到多了一个远端地址：`git remote -v`

推送项目：

`git push github master`
`git push gitee master`


总结：此方法的弊端是要多次push，建议如果要使用`pull`命令的话使用第二种，只是`push`使用第一种最方便



## Git提交到远程仓库

1.  把本地库与远程库关联：`git remote add origin 你的远程库地址`  
2. 第一次推送时：`git push -u origin master`
3. 第一次推送后，直接使用该命令即可推送修改：`git push origin master`  