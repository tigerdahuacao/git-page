---
title: 202310-JSON.stringfy的格式化
date: 2023-10-10 21:18:04
categories:
- 开发随笔
tags: 
- js
- ts
---

最近在用`ts`debug的时候, 忘记JSON.stringfy的格式化用法了, 那个缩进字符是放第二个还是第三个参数里, 我第二个参数写了`""`,结果给我报错, 重新查了查, 是在第三个参数里

![pic](./202310-JSON-stringfy的格式化/001.png)
第二个参数填`null`