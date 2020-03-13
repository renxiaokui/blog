---
title: Maven
date: 2020-03-13 14:39:34
tags: 
    - maven
categories:
    - maven
---

# Maven常用教程（待更新...）

## 镜像配置阿里云

1. 全局配置修改maven根目录下的conf文件夹中的setting.xml文件，让你下载飞速

```
<mirror>  
  <id>alimaven</id>  
  <name>aliyun maven</name>  
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
  <mirrorOf>central</mirrorOf>          
</mirror>  
```

2. 单个项目pom配置阿里云镜像仓库

   ```
   <repositories>
      <repository>
         <id>maven-ali</id>
         <url>http://maven.aliyun.com/nexus/content/groups/public//</url>
         <releases>
            <enabled>true</enabled>
         </releases>
         <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
         </snapshots>
      </repository>
   </repositories>
   ```

   