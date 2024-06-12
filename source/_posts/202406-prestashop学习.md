---
title: 202406-prestashop学习
date: 2024-06-04 10:15:29
tags:
---
下载`prestashop`, 解压, 放到网站服务器目录下, 然后访问. (这里注意,`prestashop.zip`这个文件, 它必须要通过安装`php zip extansion`插件安装, 自行解压没有作用)
![pic](./202406-prestashop学习/001.png)  
这里在windows下用`laragon`作为host环境没有作用, 我试过好几个, 包括`win server`, 都没有成功, 可能`prestashop`对于windows的支持不是很好, 我最终在linux(centos)上终于成功  
![pic](./202406-prestashop学习/002.png)
开始安装流程  
![pic](./202406-prestashop学习/003.png)  
会检查各种依赖  
![pic](./202406-prestashop学习/004.png)
安装缺失依赖  
![pic](./202406-prestashop学习/005.png)  
如果依赖都安装好了, 可以进入下一步
![pic](./202406-prestashop学习/006.png)
![pic](./202406-prestashop学习/007.png)  
配置数据库(这里是在windows上的, laragon里)
![pic](./202406-prestashop学习/008.png)
![pic](./202406-prestashop学习/009.png)
这里windows系统都会报一个很迷的错误
![pic](./202406-prestashop学习/010.png)  
Linux不会  
![pic](./202406-prestashop学习/011.jpg)
安装成功  
![pic](./202406-prestashop学习/012.jpg)