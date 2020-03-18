---
title: Redis
date: 2020-03-16 14:31:38
tags:
---

# Redis

## Redis 5 cluster集群搭建

下载：`wget wget http://download.redis.io/releases/redis-5.0.7.tar.gz`

解压：`tar -xvf  redis-5.0.7.tar.gz`

```
cd redis-5.0.7/
make PREFIX=/usr/local/redis install
make install

mkdir -p /usr/local/redis/redis-cluster
cd /usr/local/redis/redis-cluster/
mkdir 7001 7002 7003 7004 7005 7006
mkdir /var/log/redis-cluster
```

编辑配置文件：

`vi 7001/redis_7001.conf`

```
bind 0.0.0.0
#端口
port 7001
#启用集群模式
cluster-enabled yes
cluster-config-file nodes_7001.conf
#超时时间
cluster-node-timeout 5000
appendonly yes
#后台运行
daemonize yes
#非保护模式
protected-mode no
pidfile  /var/run/redis_7001.pid
#日志级别,开发选择debug，生产notice
loglevel debug
#日志文件地址
logfile /var/log/redis-cluster/redis7001.log
```

复制配置文件对应目录并修改port、cluster-config-file、pidfile和logfile：

`cp 7001/redis_7001.conf 7002/redis_7002.conf`

```
bind 0.0.0.0
#端口
port 7002
#启用集群模式
cluster-enabled yes
cluster-config-file nodes_7002.conf
#超时时间
cluster-node-timeout 5000
appendonly yes
#后台运行
daemonize yes
#非保护模式
protected-mode no
pidfile  /var/run/redis_7002.pid
#日志级别
loglevel notice
#日志文件地址
logfile /var/log/redis-cluster/redis7002.log
#设置你的密码,也可以不设置，两个保持一致即可
requirepass password
masterauth password
```

分别启动六台实例：

```
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7001/redis_7001.conf
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7002/redis_7002.conf
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7003/redis_7003.conf
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7004/redis_7004.conf
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7005/redis_7005.conf
/usr/local/redis/bin/redis-server /usr/local/redis/redis-cluster/7006/redis_7006.conf
```

查看是否启动成功：`ps -ef | grep redis `

用redis-cli创建整个redis集群(redis5以前的版本集群是依靠ruby脚本redis-trib.rb实现)：

```bash
/usr/local/redis/bin/redis-cli -a xxx --cluster create 106.13.164.226:7001 106.13.164.226:7002 106.13.164.226:7003 106.13.164.226:7004 106.13.164.226:7005 106.13.164.226:7006 --cluster-replicas 1
```

停止和重启cluster：

编辑配置：`vi /home/software/redis-5.0.7/utils/create-cluster/create-cluster`

port参数改为7000