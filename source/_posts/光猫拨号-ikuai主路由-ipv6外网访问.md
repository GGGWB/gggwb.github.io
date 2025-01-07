---
title: OpenVPN访问内网(ipv6+光猫拨号+ikuai主路由)
date: 2024-04-12 08:38:58
categories: 家庭网络
tags: 
- ikuai
- OpenVPN
- ipv6
- 内网穿透
toc: true # 是否启用内容索引
---

# 家庭网络结构

* 网络结构 ：光猫拨号 --> ikuai作为主路由分发ip

* ikuai子网网段为192.168.78.0/24

* 所有设备均有ipv6地址

# 想要实现的操作

* 操作1: 实现在外网可以通过ipv6访问家中内网设备
* 操作2: 实现ikuai开启openVPN服务端，外网客户端可以通过ipv6地址连接
* openVPN可行后，会打开ipv6防火墙，只放行特定ip，日差则使用openVPN能实现内网访问

# 遇到的问题

## 无法访问内网ipv6
子网设备获取了公网ipv6地址，但只能内网互访，外网无法连接，比方说利用手机流量，ipv6连接内网群晖失败
## 无法通过ipv6和openVPN连接内网设备
openVPN客户端连接服务端成功，可以ping通以及通过ip地址192.168.78.1访问ikuai管理页面，但无法访问ikuai子网设备

# 解决办法

先解决无法连接内网ipv6的问题

## 问题1解决方法

* 关掉光猫的ipv6防火墙（关掉以后，在ikuai设置ACL规则）

* 查看光猫背后的用户名密码并登录，不需要超级管理员

<div style="text-align:center;">
<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712853495.png" style="zoom:50%;" />
</div>

* 进入<安全>-<防火墙>-<攻击保护设置>-取消勾选**Ipv6Spi**

<div style="text-align:center;">
<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712853567.png" alt="image-20240412003925441" style="zoom:50%;" />
</div>

* 完成，外网ipv6访问全部内网设备

## 问题2现状
可以通过ipv6的openVPN连接ikuai，但不能访问内网设备
* openvpn服务端配置：

<div style="text-align:center;">
<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712904565.png" alt="img" style="zoom:50%;" />
</div>

<div style="text-align:center;">
<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712904591.png" alt="img" style="zoom:50%;" />
</div>

* openvpn客户端配置
<div style="text-align:center;">
<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712904619.png" alt="img" style="zoom:50%;" />
</div>

* 配置<认证计费>-<认证账号管理>-<账号管理>，账户名/密码随便写，openVPN客户端登录时用，其他如下

<img src="https://dsm.gwbiao.eu.org:8089/i/202404/202404121712912104.png" alt="image-20240412165502101" style="zoom:50%;" />

## 问题2解决办法：新增静态路由

* 线路：自动

* 目的地址；192.168.78.0

* 子网掩码：255.255.255.0 (24)

* 网关：192.168.78.1

## 问题分析

* ikuai的<应用工具><路由追踪>功能，做路由跟踪
  第一个：ping了内网设备192.168.78.239，第一跳为ikuai的wan口网关192.168.1.1 第二跳到外网，没有经过ikuai的lan口192.168.78.1
  第二个：ping了lan口ip 192.168.78.1，能直接跳过去
  于是判断应该是少了78.1到78.0/24网段的路由

* 当前路由表如下：
  线路1：wan1
  目的地址：0.0.0.0
  子网掩码：0.0.0.0
  网关：192.168.1.1

  线路2:sovpn（我的openVPN）
  目的地址；10.1.1.0
  子网掩码：255.255.255.0
  网关：0.0.0.0

  线路3:wan1（出外网）
  目的地址：192.168.1.0
  子网掩码：255.255.255.0
  网关：0.0.0.0

  线路3: doc_ddnsToGo（我弄的docker容器）
  192.168.11.0
  255.255.255.0
  0.0.0.0

* 于是添加78.1到78.0/24的静态路由解决问题
