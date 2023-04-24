---
title: hexo的其他一些问题
date: 2022-06-13 14:41:38
categories:
- hexo
tags: hexo学习
---

进行
```bash
hexo d
```
操作的时候, 要把代理关闭. 不然会出现 SSL的错误, 类似:
```
FATAL fatal: The remote end hung up unexpectedly
```bash
但是不连代理上GitHub又比较麻烦, 哈哈. 这个就只能看网络情况了

另外就是每次在新电脑上都没有主题,我直接把安装主题的代码放这里了,可能还是以组件的形式安装比较好,不过我都已经这么做了.
```bash
git clone https://github.com/next-theme/hexo-theme-next themes/next
```