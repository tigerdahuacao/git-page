---
title: 20240327-openwrt折腾openclash
date: 2024-03-27 11:12:40
categories:
- 教程
- 开发随笔
tags:
- openwrt
---

想给openwrt在SSR以外添加clash用于科学上网, 因为ssr节点是用来翻国内的.

但是发现`opkg update`总是出错
![pic](20240327-openwrt折腾openclash/001.png)
换了源之后依然没用
![pic](20240327-openwrt折腾openclash/002.png)
![pic](20240327-openwrt折腾openclash/003.png)
在这个帖子中找到了答案
> https://www.right.com.cn/forum/thread-199942-1-1.html
![pic](20240327-openwrt折腾openclash/004.png)
就可以正常更新了
![pic](20240327-openwrt折腾openclash/005.png)

![pic](20240327-openwrt折腾openclash/006.png)

另外, 网上一直说, 安装openclash的话, 要把这个卸载了:  
```opkg remove dnsmasq```