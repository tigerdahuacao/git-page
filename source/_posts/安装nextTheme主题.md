---
title: 安装nextTheme主题
date: 2022-07-13 17:22:11
categories:
- hexo
tags: hexo学习
---


换了台电脑执行
```bash
hexo s -g
```
后访问的页面都是白的

都是这样的错误:
> WARN  No layout: xxxxxxxxxxxx

发现是 next 主题没有安装.
官方文档参见这里

> https://theme-next.js.org/docs/getting-started/

因为我的这个工程, nextJs不是作为组件而仅仅就是一个theme. 执行这句话就可以了
```bash
git clone https://github.com/next-theme/hexo-theme-next themes/next
```
