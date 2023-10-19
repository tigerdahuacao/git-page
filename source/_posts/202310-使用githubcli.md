---
title: 使用GitHub Cli
date: 2023-10-16 10:51:23
categories:
    - 开发随笔
tags:
    - Git
---
首先使用管理员权限打开powershell, 不然会报错
![pic](./202310-使用githubcli/002.png)
安装github cli, 我指找到了这个使用winget的安装方法, 似乎没用msi版本的安装包 (应该是有的, 我在下载的过程中看到了url)
![pic](./202310-使用githubcli/001.png)
windows 11是自带winget的, 老版本的windows可能要下载, 这里忽略
```bash
winget source remove winget
winget source add winget https://mirrors.ustc.edu.cn/winget-source
winget install --id GitHub.cli
```
安装成功后, 发现命令行无法识别, 检查安装位置和path, 发现都正常
![pic](./202310-使用githubcli/005.png)
![pic](./202310-使用githubcli/003.png)
![pic](./202310-使用githubcli/004.png)
需要重启电脑就好了
![pic](./202310-使用githubcli/006.png)

首先, 必须要把代理关掉, 即使你的git config中设置了代理的socket, 但是git和GitHub其实还不是同一个东西, 所以git中的代理设置在这里没用
![pic](./202310-使用githubcli/011.png)
![pic](./202310-使用githubcli/012.png)
关掉代理后的步骤
![pic](./202310-使用githubcli/007.png)
![pic](./202310-使用githubcli/008.png)
![pic](./202310-使用githubcli/009.png)
![pic](./202310-使用githubcli/010.png)

但是GitHub Cli基本上都是帮助你完成一些仓库的操作, 新建仓库什么的.  
其依然不能解决我突然不能push代码的问题.

后来发现了问题所在, 是之前的person access token过期被自动删掉了. (虽然我创建的是永不过期, 但是Github还是时不时经常会删这些没用expiration date的token)
![pic](./202310-使用githubcli/013.png)
可以看见之前的token已经不见了

