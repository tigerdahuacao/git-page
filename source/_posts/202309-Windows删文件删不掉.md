---
title: 202309-Windows删文件删不掉
date: 2023-09-11 15:21:02
categories:
- 资源积累
tags: 
- Windows
---

这个改后缀名+批处理文件的方式, 的确有用
> https://zhuanlan.zhihu.com/p/577237417


```bat
DEL /F /A /Q \\?\%1
RD /S /Q \\?\%1
```