---
title: 利用兰空图床在群晖Docker中搭建私有图床
date: 2023-11-20 17:03:27
tags: 
- 群晖
- 图床
- 个人博客
toc: true # 是否启用内容索引
---



# 前言

目前已有的自建图床方式：

* 免费
  * PicGo + GitHub
  * Backblaze云存储+Cloudflare
* 付费
  * PicGo + 腾讯云
  * PicGo + 阿里云

准备：

1.公网 ip 地址（对于家庭网络：动态公网 ipv4 地址或者公网 ipv6 地址），没有公网 ip 的话，按下面设置好图床，也只能在内网访问了；

2.一个群晖或者服务器，用于配置为图床，必备；

介绍：

PicGo是一个很好的图片上传工具，搭配图床和写作工具typora，可以很好的完成，写作-图片插入-上传的自动化，做到一处编辑，到处查看的功能。

那么如果有**自己的群晖或者威联通或者家庭服务器**，就可以利用起自己的24小时运作的**Docker**，构建一个免费的图床，这里用到的是docker应用lsky-兰空图床。

# lsky容器的部署

## Docker界面安装

* 在群晖或者服务器新建lsky文件夹，用于存储lsky的所有数据（我这里是已安装的状态）

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713228880.png" alt="image-20240416085436244" style="zoom:50%;" />


* 在群晖docker的注册表中搜索lsky-pro-docker（https://hub.docker.com/r/halcyonazure/lsky-pro-docker） 下载安装，我这边因为可能是黑群的原因注册表经常打不开，于是我用第三方社群中下载的docker面板，先下载镜像，再创建容器，就可以曲线救国（这个docker面板其实就是portainer面板，非常好用）

* 创建容器的时候配置如下：

  * 注意本机未使用端口（例如aabb） 对应 容器端口8089端口；

  * 网络就是bridge桥接；

  * 文件夹对应关系如下：

    <img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229077.png" alt="image-20240416085752108" style="zoom:50%;" />

* 打开sky容器：http://本机IP:aabb，配置 lsky 的基础设置，选 MySQL 和 sqllite 都行

  <img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229432.png" alt="image-20240416090348475" style="zoom:50%;" />

  安装完成

  <img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229501.png" alt="image-20240416090456619" style="zoom:50%;" />

* 然后上传图片就可以了，可以继续自定义一下上传的总容量限制，**用户管理-编辑-总容量**

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229957.png" alt="image-20240416091233376" style="zoom:50%;" />

## lsky获取tokens

需要用 postman 发报文，傻瓜式操作

1.先获取接口 URL，通过这个红框可以看到你的 ip 和端口，8088 是我的，下面拼接 url 的时候要用

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713230606.png" alt="获取 url" style="zoom:30%;" />

2.打开 postman，输入如下：

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713230947.png" alt="image-20240416092903745" style="zoom:50%;" />

注意/api/v1/这部分是和 lsky 的安装版本有关系的，可以从上上图的接口 URL 里面看到，/tokens是获取给 picgo 上传图片的工具用的认证令牌，填写的 email 和 password 是发到 lsky 获取 tokens 用的身份证明

发送post 报文后，获取返回报文中的 token 字段，拼接成下面样式

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713233962.png" alt="image-20240416101916488" style="zoom:50%;" />

实际上就是在一串 token 的字母前面加了 **Bearer1｜**，拿到这个以后先保存好

* 先给lsky容器在群晖上设置反向代理，就可以在外网以https的形式访问（可选）



# PicGo配置

## 下载lankong 插件

* 插件设置这里下载lankong 插件

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229175.png" alt="image-20240416085931785" style="zoom:50%;" />

* PicGo 设置，拉到最下，勾选图床为 lankong

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713229272.png" alt="image-20240416090108524" style="zoom:50%;" />

## lankong插件配置lsky图床连接

* 在picgo 配置 lsky 图床的 server 地址，其实就是 Docker 的 ip：端口，链接形式如下图；

* auth token 就是刚才保存的 bearer 开头的一串字母；

* 其余的照图片里面填就行

  <img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713234138.png" alt="CleanShot 2024-04-16 at 10.21.42" style="zoom:50%;" />

# typora配置picgo代理上传

* 最后一步，在编写 markdown 的软件 typora 中配置插入图片自动上传图床的功能

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404161713234323.png" alt="image-20240416102520969" style="zoom:50%;" />

如图设置即可，这样在 typora 中插入图片时，会调用 picgo 自动将图片上传到 lsky 图床中进行保存，然后将图片地址发到 typora 中，通过链接的形式获取并展示。如有需要，还可以在 lsky 中设置图片上传后图片自动改名。



文章参考：

* ###### 【Docker】配置lsky pro兰空图床

https://blog.csdn.net/muxuen/article/details/129002609









