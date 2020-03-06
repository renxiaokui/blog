---
title: hexo源码管理
date: 2020-03-06 16:00:20
categories:
    - hexo
    - 源码管理
tags:
    - hexo
---

# Hexo 源码管理

_由于hexo编写完md文件后，用hexo generate编译生成对应的HTML文件，所以我们想在不同的电脑上写博客就不行了，所以采用git分支的方式，分支用来存放自己的源码，主干用来访问博客，这样就可以完美的解决自己换电脑或者电脑坏了的烦恼，下面请看具体教程_

### 新建git分支
在自己github找到博客的repository，新建一个分支
{% asset_img git.png 新建git分支 %}


### 更改仓库的默认分支

{% asset_img git2.png 修改git默认仓库 %}

点击update 确定即可

### 同步源码到github分支

到你的hexo博客目录执行命令:


    git init
    git remote add origin https://你的github源码地址
    git add .
    git commit -m 'hexo源码'
    git push origin source

### 搭建hexo环境并别写新的文章
搭建环境步骤参考的我的hexo博客搭建教程

    git clone https://你的github源码地址(此时就是你设置的源码分支的代码了)
    cd blog
    npm install hexo
    npm install hexo-deployer-git -save

恭喜！你又可以写信的博客了
