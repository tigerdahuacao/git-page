---
title: 202501-deepseek安装
date: 2025-02-06 19:26:28
categories:
- 开发随笔
tags: 
- 部署
- ai
---

## 下载
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

然后在`guff`的文件同一级目录下, 创建一个配置文件, 比如这里:
`ds-14B.txt`

```txt
# 原始文件从哪里来
FROM ./DeepSeek-R1-Distill-Qwen-14B-Q8_0.gguf    
PARAMETER temperature 0.7                       
PARAMETER top_p 0.95
PARAMETER top_k 40
PARAMETER repeat_penalty 1.1
PARAMETER min_p 0.05
PARAMETER num_ctx 1024                 
PARAMETER num_thread 4                  
PARAMETER num_gpu 8                     


# 设置对话终止符
PARAMETER stop "<｜begin▁of▁sentence｜>"
PARAMETER stop "<｜end▁of▁sentence｜>"
PARAMETER stop "<｜User｜>"
PARAMETER stop "<｜Assistant｜>"


SYSTEM """
"""

TEMPLATE """{{- if .System }}{{ .System }}{{ end }} 
{{- range $i, $_ := .Messages }} 
{{- $last := eq (len (slice $.Messages $i)) 1}}
{{- if eq .Role "user" }}<｜User｜>{{ .Content }}
{{- else if eq .Role "assistant" }}<｜Assistant｜>{{ .Content }}{{- if not $last }}<｜end▁of▁sentence｜>{{- end }}
{{- end }}
{{- if and $last (ne .Role "assistant") }}<｜Assistant｜>{{- end }} 
{{- end }}"""
```

然后使用这个`txt`的内容, 创建模型. 
```sh
ollama create mymodel-14b -f ./ds-14B.txt
```

这样, 再使用`ollama list`的时候, 就有一个`mymodel-14b`的模型会出现了

## 运行
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

## 界面
然后是在命令行中使用对话不是很美观, 可以通过`open-webui`来可视化  
> https://github.com/open-webui/open-webui.git
![pic](./202501-deepseek安装/043.png)


经过我的各种尝试, 我觉得使用`miniconda`是最优的(因为在windows下, `docker`依赖于`docker desktop`, 而`ollama`本来就很吃内存了, 再运行一个`docker desktop`卡死)
去清华源下载`miniconda`
> https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/
![pic](./202501-deepseek安装/038.png)

确认`conda`命令已经添加到系统变量
![pic](./202501-deepseek安装/039.png)

换清华的源, 可以参考:
> https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/
![pic](./202501-deepseek安装/040.png)


```yml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

创建虚拟环境:
`conda create -n open-webui python=3.11`
![pic](./202501-deepseek安装/041.png)

```bash
conda deactivate
conda activate open-webui
conda install open-webui


```bash
如果你的conda源里没有`open-webui`那么就用`pip`安装
![pic](./202501-deepseek安装/026.png)
```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

pip install open-webui
```


---
如果觉得`python`安装的方法太麻烦的话, 可以通过浏览器插件`Page Assist`的方式完成:
![pic](./202501-deepseek安装/030.png)
它会自动和`ollama`的默认端口`11434`通信. 
![pic](./202501-deepseek安装/031.png)
找到模型后张这样:  
![pic](./202501-deepseek安装/032.png)
修改显示语言
![pic](./202501-deepseek安装/033.png)
选择模型
![pic](./202501-deepseek安装/034.png)
可以使用联网功能
![pic](./202501-deepseek安装/035.png)
联网搜索设置
![pic](./202501-deepseek安装/036.png)
比如问一个新的问题, 因为`deepseek-r1`的语料库应该是在`2023年10月`的
![pic](./202501-deepseek安装/037.png)



