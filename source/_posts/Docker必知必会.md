---
title: Docker必知必会
date: 2024-01-14 21:25:01
tags:
---

# 常用命令
* 查看镜像
```bash
docker images
```
* 拉取镜像 
```bash
docker pull
```
* 删除镜像
```bash
docker rmi
```
# 创建容器
* 创建交互式容器 
```bash
docker run -it --name=c1 centos7
```
* exit 可以退出容器终端
* 查看容器信息
```bash
docker ps -a
```
* 以守护进程启动nginx
```bash
docker run -d --name=c2 nginx:1.12\(注释：斜杠换行) -p 8080:80
```
* 启动已停止的容器
```bash
docker start 名字
```
* 进入交互式容器 
```bash
docker exec -it 名字 /bin/bash
```
* 停止容器 
```bash
docker stop 名字
```
* 删除容器 
```bash
docker rm 容器名
```
# 数据卷
* 本质上是一个目录，共享目录，在创建容器时，同时将目录挂载到了容器中，删除容器不会讲数据卷中的数据删除掉 -i 交互 -t 终端 -d 守护进程
```bash
docker run -it --name=c1  -v /root/docker/data:/root/docker/data centos:7
```
# 数据卷容器
* 启动c1容器并挂载
```bash
docker run -it --name=c1 -v /volume centos:7
```
* 启动c2容器并集成c1容器的数据卷挂载关系
```bash
docker run -it --name=c2 --volumes-from c1 centos:7
```
* 查看容器的详细信息  
```bash
docker inspect c2
```
# docker部署mysql
* 启动mysql并挂载目录
```bash
docker run-id --name=test_mysql -p 3306:3306 -v /root/mysql/conf:/etc/mysql/conf.d -v /root/docker/logs:/logs -v /root/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=5201314 mysql:5.6
```
* 进入容器看一下 
```bash
docker exec -it test_mysql /bin/bash
```
# docker部署tomcat
* apache开源的web服务器
```bash
docker run -id test_tomcat -p 8080:8080 -v &pwd:/usr/local/tomcat/webapps tomcat
```
# docker镜像原理
linux一切都是文件
* 底层 bootloader+kernel=boot file system=bootfs 文件引导系统
* 二层 root file system=rootfs 例如 /etc /sbin /root /dev Linux各种发行版都是基于rootfs
* 容器创建了可读写的rootfs然后调用内核，所以容器小而快
