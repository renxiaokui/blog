---
title: fastdfs安装
date: 2020-06-08 16:52:25
tags:
---

# FastDfs安装记录简书

## FastDfs安装步骤

1. 下载

   fastdfs：wget https://github.com/happyfish100/fastdfs/archive/V6.06.tar.gz

   libfastcommon：wget https://github.com/happyfish100/libfastcommon/archive/V1.0.43.tar.gz

2.  解压安装

   ```
   tar -zxvf V6.06.tar.gz
   
   cd fastdfs-6.06/
   
   ./make.sh
   
   ./make.sh install
   ```

   tracker配置

   ```
   cd /etc/fdfs/
   
   cp tracker.conf.sample tracker.conf
   
   vi tracker.conf
   
   #修改如下配置
   #tracker存储data和log的跟路径，必须提前创建好
   base_path=/home/data/fastdfs
   port=23000
   http.server_port=80
   
   fdfs_trackerd /etc/fdfs/tracker.conf start
   ```

   storage配置

   ```
   cp storage.conf.sample storage.conf
   vi storage.conf
   #修改如下配置
   base_path=/home/data/fastdfs/storage   #storage存储data和log的跟路径，必须提前创建好
   port=23000  #storge默认23000，同一个组的storage端口号必须一致
   group_name=group1  #默认组名，根据实际情况修改
   store_path_count=1  #存储路径个数，需要和store_path个数匹配
   store_path0=/home/data/fastdfs/storage  #如果为空，则使用base_path
   tracker_server=你的ip地址:22122 
   
   fdfs_storaged /etc/fdfs/storage.conf start
   ```

3. 测试fastdfs

   ```
   echo HelloWorld > test.txt
   fdfs_upload_file /etc/fdfs/client.conf test.txt
   ```

## Nginx 整合FastDfs

下载：

wget http://nginx.org/download/nginx-1.18.0.tar.gz

wget https://github.com/happyfish100/fastdfs-nginx-module/archive/V1.22.tar.gz

```
tar -xvf nginx-1.18.0.tar.gz
tar -xvf V1.22.tar.gz
cd nginx-1.18.0/

./configure --prefix=/usr/local/nginx --with-http_ssl_module --add-module=/home/software/fastdfs/fastdfs-nginx-module-1.22/src/

make
make install

cd /home/software/fastdfs/fastdfs-6.06/conf/
cp anti-steal.jpg http.conf mime.types /etc/fdfs/

cp /home/software/fastdfs/fastdfs-nginx-module-1.22/src/mod_fastdfs.conf /etc/fdfs/

vi /etc/fdfs/mod_fastdfs.conf

#修改如下配置
tracker_server=你的ip地址:22122
base_path=/home/data/fastdfs
store_path0=/home/data/fastdfs/storage
url_have_group_name = true（配置多个tracker时，应该将此项设置为true）

```

修改nginx配置

```
vi /usr/local/nginx/conf/nginx.conf

 location /group1/M00 {
                root   /home/data/fastdfs/storage/data/;
                ngx_fastdfs_module;
        }

/usr/local/nginx/sbin/nginx -s reload
```

浏览器访问：nginx地址/测试fastdfs步骤返回的文件地址



## springboot整合fast上传

加入依赖：

```
<dependency>
    <groupId>com.github.tobato</groupId>
    <artifactId>fastdfs-client</artifactId>
    <version>1.27.2</version>
</dependency>
```

修改配置文件

```
fdfs:
  so-timeout: 5000
  connect-timeout: 5000
  #缩略图生成参数
  thumb-image:
    width: 150
    height: 150
  #TrackerList参数,支持多个
  tracker-list:
    - fastdfs服务器的ip:22122
  web-server-url: 'web访问地址，一般是nginx的域名'
```

上传文件：

```
    @Autowired
    private FastFileStorageClient fastFileStorageClient;
    
    #调用api
    fastFileStorageClient.uploadFile()
```

