---
title: Nginx
date: 2020-03-12 11:54:14
tags: 
    - nginx
categories:
    - nginx
---

# Nginx

## 安装

1. 基础环境： `yum -y install gcc-c++ pcre  pcre-devel zlib  zlib-devel openssl openssl-devel`

2. 下载 `wget http://nginx.org/download/nginx-1.16.1.tar.gz`

3. 解压 `tar -xvf nginx-1.16.1.tar.gz`

4. 进入解压目录：`cd nginx-1.16.1/`

5. 编译安装：

   `./configure --prefix=/usr/local/nginx`
   `make && make install`

6. 启动nginx

   `cd /usr/local/nginx/sbin`

   `./nginx`

7.  打开你的浏览器访问ip即可

   

   ## 常用编译参数

   编译时`./configure --help`以--without开头的都默认安装

   ```
   ./configure \
   --prefix=/usr/local/nginx \
   --pid-path=/var/run/nginx/nginx.pid \
   --lock-path=/var/lock/nginx.lock \
   --error-log-path=/var/log/nginx/error.log \
   --http-log-path=/var/log/nginx/access.log \
   --with-http_gzip_static_module \
   --http-client-body-temp-path=/var/temp/nginx/client \
   --http-proxy-temp-path=/var/temp/nginx/proxy \
   --http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
   --http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
   --http-scgi-temp-path=/var/temp/nginx/scgi \
   --with-http_stub_status_module \
   --with-http_ssl_module \
   --with-file-aio \
   --with-http_realip_module
   ```

   

   ## 简单命令

   查看进程 `ps -ef|grep nginx`

   ```
   查看版本信息：/usr/local/nginx/sbin/nginx -V
   启动：/usr/local/nginx/sbin/nginx
   停止：/usr/local/nginx/sbin/nginx -s stop
   优雅停止:/usr/local/nginx/sbin/nginx -s quit
   重启:/usr/local/nginx/sbin/nginx -s reopen 
   查看配置是否正确：/usr/local/nginx/sbin/nginx -t
   重载配置：/usr/local/nginx/sbin/nginx -s reload
   指定配置文件启动：/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
   ```

   

   ## 配置详解

   ### 全局变量

   ```
   #Nginx的worker进程运行用户以及用户组
   #user  nobody nobody;
   #Nginx开启的进程数,一般有几个cpu就设置几,或者是N-1
   worker_processes  1;
   #worker_processes 4     #4核CPU 
   #worker_cpu_affinity 0001 0010 0100 1000;  
   #定义全局日志级别，[debug|info|notice|warn|crit]
   #error_log  logs/error.log  info;
   #指定进程ID存储文件位置
   #pid  logs/nginx.pid;
   ```

   ###  事件

   ```
   events {
       #use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
       use epoll;
       #每个进程可以处理的最大连接数，理论上每台nginx服务器的最大连接数为worker_processes*worker_connections。理论值：worker_rlimit_nofile/worker_processes
       #注意：最大客户数也由系统的可用socket连接数限制（~ 64K），所以设置不切实际的高没什么好处
       worker_connections  65535;    
       #worker工作方式：串行（一定程度降低负载，但服务器吞吐量大时，关闭使用并行方式）
       #multi_accept on; 
   }
   ```

   ### http

   ```
      #文件扩展名与文件类型映射表
       include mime.types;
       #默认文件类型
       default_type application/octet-stream;
    
   #日志相关定义
       #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
       #                  '$status $body_bytes_sent "$http_referer" '
       #                  '"$http_user_agent" "$http_x_forwarded_for"';
       #定义日志的格式。后面定义要输出的内容。
       #1.$remote_addr 与$http_x_forwarded_for 用以记录客户端的ip地址；
       #2.$remote_user ：用来记录客户端用户名称；
       #3.$time_local ：用来记录访问时间与时区；
       #4.$request  ：用来记录请求的url与http协议；
       #5.$status ：用来记录请求状态； 
       #6.$body_bytes_sent ：记录发送给客户端文件主体内容大小；
       #7.$http_referer ：用来记录从那个页面链接访问过来的；
       #8.$http_user_agent ：记录客户端浏览器的相关信息
       #连接日志的路径，指定的日志格式放在最后。
       #access_log  logs/access.log  main;
       #只记录更为严重的错误日志，减少IO压力
       error_log logs/error.log crit;
       #关闭日志
       #access_log  off;
    
       #默认编码
       #charset utf-8;
       #服务器名字的hash表大小
       server_names_hash_bucket_size 128;
       #客户端请求单个文件的最大字节数
       client_max_body_size 8m;
       #指定来自客户端请求头的hearerbuffer大小
       client_header_buffer_size 32k;
       #指定客户端请求中较大的消息头的缓存最大数量和大小。
       large_client_header_buffers 4 64k;
       #开启高效传输模式。
       sendfile on;
       #防止网络阻塞
       tcp_nopush on;
       tcp_nodelay on;    
       #客户端连接超时时间，单位是秒
       keepalive_timeout 60;
       #客户端请求头读取超时时间
       client_header_timeout 10;
       #设置客户端请求主体读取超时时间
       client_body_timeout 10;
       #响应客户端超时时间
       send_timeout 10;
    
   #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。
       fastcgi_connect_timeout 300;
       fastcgi_send_timeout 300;
       fastcgi_read_timeout 300;
       fastcgi_buffer_size 64k;
       fastcgi_buffers 4 64k;
       fastcgi_busy_buffers_size 128k;
       fastcgi_temp_file_write_size 128k;
    
   #gzip模块设置
       #开启gzip压缩输出
       gzip on; 
       #最小压缩文件大小
       gzip_min_length 1k; 
       #压缩缓冲区
       gzip_buffers 4 16k;
       #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
       gzip_http_version 1.0;
       #压缩等级 1-9 等级越高，压缩效果越好，节约宽带，但CPU消耗大
       gzip_comp_level 2;
       #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
       gzip_types text/plain application/x-javascript text/css application/xml;
       #前端缓存服务器缓存经过压缩的页面
       gzip_vary on;
   ```

   ###  include

   ```
   #引入外部配置文件，避免单个文件过大 例如：include 项目1.conf
   include mime.types;
   ```

   ### server

   ```
       server {
           #监听端口
           listen       80;
           #访问域名
           server_name  localhost;
           #编码格式，若网页格式与此不同，将被自动转码
           #charset koi8-r;
           #虚拟主机访问日志定义
           #access_log  logs/host.access.log  main;
           #对URL进行匹配
           location / {
               #访问路径，可相对也可绝对路径
               root   html;
               #首页文件。以下按顺序匹配
               index  index.html index.htm;
           }
   		#错误信息返回页面
           error_page   500 502 503 504  /50x.html;
           location = /50x.html {
               root   html;
           }
       }
   #HTTPS虚拟主机定义
       # HTTPS server
       #
       #server {
       #    listen       443 ssl;
       #    server_name  localhost;
       #    ssl_certificate      cert.pem;
       #    ssl_certificate_key  cert.key;
       #    ssl_session_cache    shared:SSL:1m;
       #    ssl_session_timeout  5m;
       #    ssl_ciphers  HIGH:!aNULL:!MD5;
       #    ssl_prefer_server_ciphers  on;
       #    location / {
       #        root   html;
       #        index  index.html index.htm;
       #    }
       #}
   ```

   ### 状态监控

   ```
   #Nginx运行状态，StubStatus模块获取Nginx自启动的工作状态（编译时要开启对应功能）
           #location /NginxStatus {
           #    #启用StubStatus的工作访问状态    
           #    stub_status    on;
           #    #指定StubStaus模块的访问日志文件 可off
           #    access_log    logs/Nginxstatus.log;
           #    #Nginx认证机制（需Apache的htpasswd命令生成）
           #    #auth_basic    "NginxStatus";
           #    #用来认证的密码文件
           #    #auth_basic_user_file    ../htpasswd;    
           #}
   ```

   ### 负载均衡

   ```
   #负载均衡服务器池
   upstream my_loadbalance {
       #调度算法
       #1.轮循（默认）（weight轮循权值）
       #2.ip_hash：根据每个请求访问IP的hash结果分配。（会话保持）
       #3.fair:根据后端服务器响应时间最短请求。（upstream_fair模块）
       #4.url_hash:根据访问的url的hash结果分配。（需hash软件包）
       #参数：
       #down：表示不参与负载均衡
       #backup:备份服务器
       #max_fails:允许最大请求错误次数
       #fail_timeout:请求失败后暂停服务时间。
       server 192.168.231.136 weight=1 max_fails=2 fail_timeout=30;
       server 192.168.231.137 weight=2 max_fails=2 fail_timeout=30;
   }
   #负载均衡调用
   server {
       ...
       location / {
       proxy_pass http://my_loadbalance;
       }
   }
   ```

   ### URL重写

   ```
   #根据不同的浏览器URL重写
   if($http_user_agent ~ Firefox){
   rewrite ^(.*)$  /firefox/$1 break; 
   }
   if($http_user_agent ~ MSIE){
   rewrite ^(.*)$  /msie/$1 break; 
   }
    
   #实现域名跳转
   location / {
       rewrite ^/(.*)$ https://web8.example.com$1 permanent;
   }
   ```

   ###  IP黑名单

   ```
   #限制IP访问
   location / {
       deny 192.168.0.2；
       allow 192.168.0.0/24;
       allow 192.168.1.1;
       deny all;
   }
   ```

   

   ## 动态添加模块

   1. 停止nginx `/usr/local/nginx/sbin/nginx -s stop`
   2. 添加编译模块：`./configure --with-http_ssl_module`
   3. 编译：`make`（注意：这里不用make install重新安装，会把之前的nginx重新覆盖）
   4. 备份之前的nginx：`cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak`
   5. 更换新的nginx：` cp objs/nginx /usr/local/nginx/sbin/`
   6. 查看是否安装成功：`/usr/local/nginx/sbin/nginx -V`

   

   ## 配置https

   1. 查看是否有ssl模块功能：`/usr/local/nginx/sbin/nginx -V` 没有请安装http_ssl_module

   2. 上传你的ssl证书

   3. 配置nginx https

      ```
       	server {
              listen       80;
              server_name  www.domain.com;
             location / {
                  rewrite ^(.*)$ https://$host$1 permanent; #把http的域名请求转成https
              }
          }
      
          server {
              listen       443 ssl;
              server_name  www.domain.com;
      
              ssl_certificate /usr/local/ssl/nginx.crt; #证书crt
              ssl_certificate_key /usr/local/ssl/nginx.key;#证书key
      
              ssl_session_cache    shared:SSL:1m;
              ssl_session_timeout  5m;
      
              ssl_ciphers  HIGH:!aNULL:!MD5;
              ssl_prefer_server_ciphers  on;
      
              location / {
                  root   html; 
                  index  index.html index.htm;
              }
              error_page   500 502 503 504  /50x.html;
              location = /50x.html {
                  root   html;
              }
          }
      ```

      

   4. 检查配置正确性：`/usr/local/nginx/sbin/nginx -t`

   5. 重新加载nginx配置：`/usr/local/nginx/sbin/nginx -s reload`

   6. 恭喜！你现在可以用https访问你的网站了

   

   ## 日志切割

   1. 定时任务命令

      查看状态：`service crond status`

      开启：`service crond start`

      关闭：`service crond stop`

      重启：`service crond restart`

      重载配置：`service crond reload`

      编辑任务列表：`crontab -e`

      查看任务列表：`crontab -l`

      删除任务：`crontab -r`

      如果定时任务不存在则安装：`yum -y install crontabs`

   2. 编写日志切割脚本文件

      `vi /home/cut_nginx_log.sh`

      ```
      #!/bin/bash
      logs_path="/var/log/nginx/" #nginx日志路径
      pid_path="/var/run/nginx/nginx.pid" #nginx的pid
      
      mv ${logs_path}access.log ${logs_path}access_$(date -d "yesterday" +"%Y%m%d").log
      mv ${logs_path}error.log ${logs_path}error_$(date -d "yesterday" +"%Y%m%d").log
      kill -USR1 `cat ${pid_path}`
      ```

   3.  修改权限 `chmod +x cut_nginx_log.sh`

   4. 建立定时任务：

      `crontab -e `

      ```
      0 0 * * * /bin/bash /home/cut_nginx_log.sh
      ```

      重启任务：`service crond restart`

## keepalived+nginx高可用

### 安装Keepalived

下载：`wget https://www.keepalived.org/software/keepalived-2.0.20.tar.gz`

```
tar -xvf keepalived-2.0.20.tar.gz
cd keepalived-2.0.20/
./configure --prefix=/usr/local/keepalived --sysconf=/etc
make && make install
```

### 配置解读

```
global_defs { --- 全局配置部分
notification_email { --- 设置发送邮件信息的收件人
acassen@firewall.loc
failover@firewall.loc
sysadmin@firewall.loc
}
notification_email_from oldboy@163.com --- 设置连接的邮件服务器信息
smtp_server 163.smtp.xxx_
smtp_connect_timeout 30 重新连接邮箱的时间
router_id LVS_DEVEL --- 高可用集群主机身份标识(集群中主机身份标识名称不能重复)
}

vrrp_instance VI_1 { 
state MASTER --- 标识所在家族中的身份 (MASTER/BACKUP)
interface eth0 --- 指定虚拟IP地址出现在什么网卡上
virtual_router_id 51 --- 标识家族身份信息 多台高可用服务配置要一致
priority 100 --- 设定优先级 优先级越高,就越有可能成为主
advert_int 1 --- 定义组播包发送的间隔时间(秒) 主和备配置一样 1
authentication { --- 实现通讯需要有认证过程
auth_type PASS
auth_pass 1111
}
virtual_ipaddress { --- 配置虚拟IP地址信息
192.168.200.16
192.168.200.17
192.168.200.18
}
}
```

### 高可用双主热备配置

*其他的配置可以全部删除*

原理： 域名解析-->>dns服务器-->>  不同的服务器-->>nginx

查看自己的网卡：`ip addr`（我的是eth0）

`vi /etc/keepalived/keepalived.conf`

node01配置：

```
global_defs {
router_id node01
}

vrrp_instance VI_1 {
state MASTER
interface eth0
virtual_router_id 51
priority 100
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.16.5 
}
}

vrrp_instance VI_2 {
state BACKUP
interface eth0
virtual_router_id 52
priority 80
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.16.6
}
}
```

node02配置：

```
global_defs {
router_id node02
}

vrrp_instance VI_1 {
state BACKUP
interface eth0
virtual_router_id 51
priority 80
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.16.5 
}
}

#BACKUP配置
vrrp_instance VI_2 {
state MASTER
interface eth0
virtual_router_id 52
priority 100
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.16.6
}
}
```

### 注册keepalived为系统服务

```
cp keepalived/etc/init.d/keepalived /etc/init.d/
cp keepalived/etc/sysconfig/keepalived /etc/sysconfig/
```

启动：`systemctl start keepalived.service`

停止：`systemctl stop keepalived.service`

开机自启：`systemctl enable keepalived.service`

关闭开机自启：`systemctl disable keepalived.service`

### nginx心跳检测，实现7*24小时服务

编写脚本：`vi /etc/keepalived/nginx_check.sh`

```
#!/bin/bash
#判断nginx是否宕机，宕机了尝试重启
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];then
/usr/local/nginx/sbin/nginx
#三秒后重启，没有成功则停止keepalived,启动备用机
sleep 3
if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
killall keepalived
fi
fi
```

修改脚本权限：`chmod +x /etc/keepalived/nginx_check.sh`

加到keepalived.conf中使其生效，放到vrrp_instance配置上面即可：

```
global_defs {
router_id node01
}
#编写脚本
vrrp_script check_ng {
script " /etc/keepalived/check_nginx.sh "
interval 2 #每隔2s执行一次
weight 2 #执行成功，权重加2
}

vrrp_instance VI_1 {
state MASTER
interface eth0
virtual_router_id 51
priority 100
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.16.5 
}
#执行脚本
track_script {
check_ng
}
}
```

##   lvs + keepalived + nginx

1. 在lvs主和备安装ipvsadm ：`yum install ipvsadm -y`

2. 在lvs主和备安装keepalived，教程在上面

3. realserver机器编写脚本

   `cd /etc/init.d/`

   `vim realserver`

   ```
   #虚拟的vip 根据自己的实际情况定义
   SNS_VIP=192.168.1.200
   /etc/rc.d/init.d/functions
   case "$1" in
   start)
          ifconfig lo:0 $SNS_VIP netmask 255.255.255.255 broadcast $SNS_VIP
          /sbin/route add -host $SNS_VIP dev lo:0
          echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
          echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
          echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
          echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce
          sysctl -p >/dev/null 2>&1
          echo "RealServer Start OK"
          ;;
   stop)
          ifconfig lo:0 down
          route del $SNS_VIP >/dev/null 2>&1
          echo "0" >/proc/sys/net/ipv4/conf/lo/arp_ignore
          echo "0" >/proc/sys/net/ipv4/conf/lo/arp_announce
          echo "0" >/proc/sys/net/ipv4/conf/all/arp_ignore
          echo "0" >/proc/sys/net/ipv4/conf/all/arp_announce
          echo "RealServer Stoped"
          ;;
   *)
          echo "Usage: $0 {start|stop}"
          exit 1
   esac
   exit 0
   ```

   `chmod +x /etc/init.d/realserver`

   查看结果：`ip a` lo:下有个自己设置的虚拟ip192.168.1.200

4. 配置keepalived

   `vim /etc/keepalived/keepalived.conf`

   ```
   global_defs {
      router_id LVS_DEVEL  # 设置lvs的id，在一个网络内应该是唯一的
   }
   vrrp_instance VI_1 {
       state MASTER   #指定Keepalived的角色，MASTER为主，BACKUP为备 记得大写
       interface eth0  #网卡id 不同的电脑网卡id会有区别 可以使用:ip a查看
       virtual_router_id 51  #虚拟路由编号，主备要一致
       priority 100  #定义优先级，数字越大，优先级越高，主DR必须大于备用DR
       advert_int 1  #检查间隔，默认为1s
       authentication {   #这里配置的密码最多为8位，主备要一致，否则无法正常通讯
           auth_type PASS
           auth_pass 1111
       }
       virtual_ipaddress {
           192.168.1.200  #定义虚拟IP(VIP)为192.168.1.200，可多设，每行一个
       }
   }
   # 定义对外提供服务的LVS的VIP以及port，负载均衡
   virtual_server 192.168.1.200 80 {
       delay_loop 6 # 设置健康检查时间，单位是秒
       lb_algo rr # 设置负载调度的算法为wlc
       lb_kind DR # 设置LVS实现负载的机制，有NAT、TUN、DR三个模式
       nat_mask 255.255.255.0
       persistence_timeout 0
       protocol TCP
       real_server 192.168.1.130 80 {  # 指定real server1的IP地址，可以使web或者nignx服务器
           weight 3   # 配置节点权值，数字越大权重越高
           TCP_CHECK {
           connect_timeout 10
           nb_get_retry 3
           delay_before_retry 3
           connect_port 80
           }
       }
       real_server 192.168.1.131 80 {  # 指定real server2的IP地址，可以使web或者nignx服务器
           weight 3  # 配置节点权值，数字越大权重越高
           TCP_CHECK {
           connect_timeout 10
           nb_get_retry 3
           delay_before_retry 3
           connect_port 80
           }
        }
   }
   ```

5. 配置备用机器

   ```
   global_defs {
      router_id LVS_DEVEL  # 设置lvs的id，在一个网络内应该是唯一的
   }
   vrrp_instance VI_1 {
       state BACKUP #指定Keepalived的角色，MASTER为主，BACKUP为备 记得大写
       interface eno16777736  #网卡id 不同的电脑网卡id会有区别 可以使用:ip a查看
       virtual_router_id 51  #虚拟路由编号，主备要一致
       priority 50  #定义优先级，数字越大，优先级越高，主DR必须大于备用DR
       advert_int 1  #检查间隔，默认为1s
       authentication {   #这里配置的密码最多为8位，主备要一致，否则无法正常通讯
           auth_type PASS
           auth_pass 1111
       }
       virtual_ipaddress {
           192.168.1.200  #定义虚拟IP(VIP)为192.168.1.200，可多设，每行一个
       }
   }
   # 定义对外提供服务的LVS的VIP以及port
   virtual_server 192.168.1.200 80 {
       delay_loop 6 # 设置健康检查时间，单位是秒
       lb_algo rr # 设置负载调度的算法为wlc
       lb_kind DR # 设置LVS实现负载的机制，有NAT、TUN、DR三个模式
       nat_mask 255.255.255.0
       persistence_timeout 0
       protocol TCP
       real_server 192.168.1.130 80 {  # 指定real server1的IP地址
           weight 3   # 配置节点权值，数字越大权重越高
           TCP_CHECK {
           connect_timeout 10
           nb_get_retry 3
           delay_before_retry 3
           connect_port 80
           }
       }
       real_server 192.168.1.131 80 {  # 指定real server2的IP地址
           weight 3  # 配置节点权值，数字越大权重越高
           TCP_CHECK {
           connect_timeout 10
           nb_get_retry 3
           delay_before_retry 3
           connect_port 80
           }
        }
   }
   ```

6.  lvs常用命令

   查看当前配置的虚拟服务和各个RS的权重：`ipvsadm -Ln`

   查看当前ipvs模块中记录的连接（可用于观察转发情况）：`ipvsadm -lnc`

   查看ipvs模块的转发情况统计：`ipvsadm -Ln --stats | --rate`

   查看lvs的超时时间：`ipvsadm -L --timeout`

   设置超时时间：`ipvsadm --set 1 10 300`

