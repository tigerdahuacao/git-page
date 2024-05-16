---
title: 202405-补档阿里云服务器的默认系统(非Centos)安装podman
date: 2024-05-16 18:06:38
categories:
- 开发随笔
tags: 
- 部署
- Docker
- podman
---
在阿里云定制的linux系统上想安装docker会报错
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/001.png)  
只能安装podman(已经自带了)
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/002.png)
使用podman来运行docker的docker-compose文件发现报错
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/003.png)
查了一下, 要用一个 podman compose命令, 其实是`podman-compose`  
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/004.png)
这其实是一个python的命令, 安装python, 再用pip安装
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/005.png)
用`podman-compose` 把docker工程启动成功
![pic](./202405-补档阿里云服务器的默认系统-非Centos-安装podman/006.png)

不要忘了去防火墙开放端口哦, 图中可以看到都是`3333`