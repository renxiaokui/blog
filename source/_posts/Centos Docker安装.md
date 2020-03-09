---
title: Centos Docker安装
date: 2020-03-06 17:51:34
tags: 
    - linux
    - centos
    - docker
categories:
    - docker
---

### Centos7 Docker安装

#### 安装docker

    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    sudo yum makecache fast
    sudo yum -y install docker-ce
    sudo systemctl enable docker
    sudo systemctl start docker


#### 修改docker镜像源：
这里找个几个速度比较快的：
1. Docker 官方中国区
https://registry.docker-cn.com
2. 网易
http://hub-mirror.c.163.com
3. ustc
https://docker.mirrors.ustc.edu.cn

修改配置：`vi /etc/docker/daemon.json`

    {
      "registry-mirrors": ["http://hub-mirror.c.163.com"]
    }

这里推荐修改成阿里云的源：进入到阿里云控制台找到镜像服务，执行该脚本即可，每个区域不一样，所以这里就不贴出来相关代码了

{% asset_img docker-mirror.png 阿里云的源 %}

#### 安装docker-compose

    yum -y install epel-release
    yum -y install python-pip
    pip install --upgrade pip
    pip install docker-compose

验证是否安装成功

    docker -v
