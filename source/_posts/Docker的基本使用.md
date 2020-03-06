---
title: Docker的基本使用
date: 2020-03-06 18:02:14
tags: 
    - linux
    - centos
    - docker
categories:
    - docker
---
## Docker的基本使用

### Docker的管理命令

设置开机自启:  `systemctl enable docker`
启动docker： `systemctl start docker`
重启docker： `systemctl restart docker`
守护进程重启: `systemctl daemon-reload`
停止docker： `systemctl stop docker`

### Docker的基本使用命令
查看docker中所有的镜像：`docker images`
查看正在运行的容器：`docker ps`
查看所有容器：`docker ps -a`
拉取docker镜像：`docker pull xxx`
启动镜像： `docker run xxx`
常用参数： -d 守护进程运行 -p 端口映射 --name 启动后容器的名字 -v 挂载存储卷 
--restart="no"
指定容器停止后的重启策略（no：容器退出时不重启  on-failure：容器故障退出（返回值非零）时重启 always：容器退出时总是重启）
停止容器：docker stop [container]
进入docker容器内部：`docker exec -it [container] /bin/bash`
查看启动容器日志:`docker logs -f`
删除容器：`docker rm [container](或者name)`
删除所有容器：`docker rm $(docker ps -a -q)`
上传镜像到仓库`docker push [image]`
容器打包成新的镜像:`docker commit [container] newImage`
保存镜像：`docker save -o  /home/xxx.tar [container]`
加载镜像： `docker load -i xxx.tar`
删除镜像:`docker rmi -f [image]`

### Docker仓库的基本使用
可搭建docker自带仓库Docker Registry或者Habor
本地连接仓库

    vi /etc/docker/daemon.json
    "insecure-registries":["192.168.10.128:5000"]
    systemctl daemon-reload
    systemctl restart docker

登录私有仓库

    docker login 192.168.10.128:5000
    docker tag [image]  192.168.10.128:5000 /[image]:tag
    docker push 192.168.10.128:5000 /[image]:tag


