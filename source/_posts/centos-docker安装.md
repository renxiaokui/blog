---
title: centos_docker安装
date: 2020-03-06 17:51:34
tags: 
    - linux
    - centos
    - docker
categories:
    - docker
---

### Centos7 Docker安装

安装docker

    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    sudo yum makecache fast
    sudo yum -y install docker-ce
    sudo systemctl enable docker
    sudo systemctl start docker

安装docker-compose

    yum -y install epel-release
    yum -y install python-pip
    pip install --upgrade pip
    pip install docker-compose

验证是否安装成功

    docker -v
