---
title: 202501-deepseek安装
date: 2025-02-06 19:26:28
categories:
- 开发随笔
tags: 
- 部署
- ai
---
下载并安装`ollama`
![pic](./202501-deepseek安装/001.png)
安装
![pic](./202501-deepseek安装/002.png)
安装完成后自动退出安装界面, 并且会隐藏到系统托盘中
![pic](./202501-deepseek安装/003.png)
在命令行中, 查看`ollama`命令是否存在
![pic](./202501-deepseek安装/004.png)

也可以在浏览器中访问`http://localhost:11434/`来查看`ollama`是否已经正常运行
![pic](./202501-deepseek安装/012.png)

此外, 安装完`ollama`后, 它会开机启动. 通过把启动项删掉的方法, 取消开机启动
`C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
![pic](./202501-deepseek安装/018.png)

搜索你想要安装的模型, 并且下载对应的版本, 我这里16G的4060ti, 最好安装的9G的版本, 通常来说, 如果显存装不下, 那么会占用一定的内存来弥补.
![pic](./202501-deepseek安装/005.png)

执行命令, 这里`run`命令是下载并运行, `pull`命令是下载
![pic](./202501-deepseek安装/006.png)
默认情况下, 它的模型下载特别慢. 我花了差不多4个小时, 主要是下了一会儿就会停顿住  
停顿的话, 可以`ctrl+c`终止脚本, 然后重新pull, 它会断点续传的,如下图
![pic](./202501-deepseek安装/008.png)
终于将要下载完毕
![pic](./202501-deepseek安装/013.png)
下载完毕后, 它会进行`sha256`校验文件完整性
![pic](./202501-deepseek安装/014.png)
如果校验通过, 会出现`success`.
![pic](./202501-deepseek安装/015.png)

此外还有一个下载模型的方法, 如果你的网络实在是差.可以去阿里的魔搭社区`https://www.modelscope.cn/models/`下载模型文件. 
![pic](./202501-deepseek安装/009.png)
要注意, 这里选择`GUFF`格式的模型, 否则`ollama`不认识, 还需要做额外的模型转换步骤
![pic](./202501-deepseek安装/010.png)
挑一个量化模型下载,这里的`Qxx`是各种量化程度, 不过这个网站的下载速度不快, 所以这里选择一个`14b`的模型(对应9GB显存)
![pic](./202501-deepseek安装/011.png)

通过`ollama`run命令运行模型.
`ollama run deepseek-r1:32b`
![pic](./202501-deepseek安装/016.png)
会发现, 显存被完全占有, 然后内存占用也很多. 模型的输出有明显卡顿
![pic](./202501-deepseek安装/017.png)
彻底退出`ollama`,也就是把任务栏托盘的羊驼退出后, 资源才会被释放, 光光在命令行里结束会话没用.
![pic](./202501-deepseek安装/019.png)

此外, `ollama`没用配置, 模型文件又很大. 要怎么才能转移文件位置呢?  
`ollama`的文件位置放在: `C:\Users\Administrator\.ollama`
![pic](./202501-deepseek安装/020.png)  
可以看到, 这个文件很大, 就是我们的模型文件  
![pic](./202501-deepseek安装/021.png)
把模型移到你想放置的磁盘的位置
![pic](./202501-deepseek安装/022.png)
然后配置环境变量,`win+r`打开运行窗口, 输入`rundll32 sysdm.cpl,EditEnvironmentVariables`  
![pic](./202501-deepseek安装/023.png)
更多环境变量可以参考: 
> https://github.com/datawhalechina/handy-ollama/blob/main/docs/C2/2.%20Ollama%20%E5%9C%A8%20Windows%20%E4%B8%8B%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%85%8D%E7%BD%AE.md
![pic](./202501-deepseek安装/024.png)
![pic](./202501-deepseek安装/025.png)

如果要删除模型, 使用:  
`ollama rm deepseek-r1:32b`
![pic](./202501-deepseek安装/027.png)
事实证明, 16G的显存运行`32b`的模型, 太卡了. 还是安装`14b`的模型比较好
![pic](./202501-deepseek安装/028.png)
![pic](./202501-deepseek安装/029.png)

---
然后是在命令行中使用对话不是很美观, 安装`open-webui`  
`git clone https://github.com/open-webui/open-webui.git`
![pic](./202501-deepseek安装/026.png)



