---
title: java线上排查
date: 2020-05-14 15:54:26
tags:
---

# 使用 jstack 分析 CPU 问题

1. 使用`jsp`命令查看java服务的pid
2. 用`top -H -p pid`来找到 CPU 使用率比较高的一些线程
3. 然后将占用最高的 pid 转换为 16 进制`printf '%x\n' pid`得到 nid
4. `jstack pid |grep 'nid' -C5 –color`