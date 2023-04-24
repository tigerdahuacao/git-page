---
title: 前端面试基础-cookie HTTPS 安全性等
date: 2023-01-09 17:07:06
categories:
- 开发随笔
- 教程
tags: 
- js
- HTTP
---

### 浏览器缓存
缓存相关字段:
1. expires
    有点过时的属性, 被cache-control替代, 表示一个绝对的过期时间.
    如果在请求资源时, 
2. cache-control
3. etag/if-none-match
4. last-modified/if-modified-since
强缓存:

协商缓存:

浏览器先检查强缓存, 再检查协商缓存

参考了这里: 
> https://space.bilibili.com/15445514/channel/collectiondetail?sid=814942