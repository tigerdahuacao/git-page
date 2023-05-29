---
title: 202305-离线安装python第三方库
date: 2023-05-12 10:22:16
categories:
- 开发随笔
tags: 
- python
---

//TODO 未写完

为了可以在vscode的ipynotebook中使用debug功能, 必须要安装6.0版本以上的ipykernel

```python
import sys
import ipykernel

# ensure that ipykernel version 6.0.0 or greater
print("kernal version:",ipykernel.__version__)

# Ensure that you're using a kernel based on Python 3.7 or greater.	
print("Python version:",sys.version)	
```

> https://github.com/microsoft/vscode-jupyter/wiki/Setting-Up-Run-by-Line-and-Debugging-for-Notebooks


首先是碰到的第一个问题, 因为公司电脑权限的问题, 在安装时候提示没有权限. 添加 `--user` 来解决
> https://blog.csdn.net/zhouzhiwengang/article/details/120011564

但是还是不尽人意, 尝试通过手动安装
> https://blog.csdn.net/m_buddy/article/details/86619440
> https://zhuanlan.zhihu.com/p/351494670
> https://www.zhihu.com/question/334548570
