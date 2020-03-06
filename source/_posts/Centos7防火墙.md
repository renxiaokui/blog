title: centos7防火墙设置
author: 阿奎
tags: []
categories: []
date: 2020-03-04 15:42:00
---

查看centos系统版本: `cat /etc/issue`

启动: `systemctl start firewalld`

关闭: `systemctl stop firewalld`

查看状态: `systemctl status firewalld`

开机禁用: `systemctl disable firewalld`

开机启用: `systemctl enable firewalld`

显示状态: `firewall-cmd --state`

查看所有打开的端口: `firewall-cmd --zone=public --list-ports`

添加: `firewall-cmd --zone=public --add-port=80/tcp --permanent`

重新载入: `firewall-cmd --reload`

查看: `firewall-cmd --zone= public --query-port=80/tcp`

删除: `firewall-cmd --zone= public --remove-port=80/tcp --permanent`