---
title: 202406-prestashop学习
date: 2024-06-04 10:15:29
categories:
- 开发随笔
tags: 
- 部署
- prestashop
---

## 安装部署

下载`prestashop`, 解压, 放到网站服务器目录下, 然后访问. (这里注意,`prestashop.zip`这个文件, 它必须要通过安装`php zip extansion`插件安装, 自行解压没有作用)
![pic](./202406-prestashop学习/001.png)  
这里在windows下用`laragon`作为host环境没有作用, 我试过好几个, 包括`win server`, 都没有成功, 可能`prestashop`对于windows的支持不是很好, 我最终在linux(centos)上终于成功  
![pic](./202406-prestashop学习/002.png)
开始安装流程  
![pic](./202406-prestashop学习/003.png)  
会检查各种依赖  
![pic](./202406-prestashop学习/004.png)
安装缺失依赖  
![pic](./202406-prestashop学习/005.png)  
如果依赖都安装好了, 可以进入下一步
![pic](./202406-prestashop学习/006.png)
![pic](./202406-prestashop学习/007.png)  
配置数据库(这里是在windows上的, laragon里)
![pic](./202406-prestashop学习/008.png)
![pic](./202406-prestashop学习/009.png)
这里windows系统都会报一个很迷的错误
![pic](./202406-prestashop学习/010.png)  
在Linux不会报错  
![pic](./202406-prestashop学习/011.jpg)
安装成功      
![pic](./202406-prestashop学习/012.jpg)  
访问后台会告诉你为了安全考虑, 需要删除`/Install`文件夹
![pic](./202406-prestashop学习/015.png)
![pic](./202406-prestashop学习/014.png)  
删除后可以进入
![pic](./202406-prestashop学习/016.png)

这里有一点, 一定要启用SSL, 或者在店铺的后台管理设置中也启用SSL, 否则会导致访问店铺时出现一直重定向的问题.
![pic](./202406-prestashop学习/017.png)
---
## 插件安装
添加`PayPal Offical`插件 
![pic](./202406-prestashop学习/018.png)
![pic](./202406-prestashop学习/019.png)  
当然也可以通过从插件市场下载, 然后通过上传的方式安装  
> https://addons.prestashop.com/en/payment-card-wallet/1748-paypal-official.html
![pic](./202406-prestashop学习/020.png)
![pic](./202406-prestashop学习/021.png)
![pic](./202406-prestashop学习/0211.png)
![pic](./202406-prestashop学习/022.png)  
![pic](./202406-prestashop学习/0212.png)  
但是事实上我发现通过UI界面安装这个`PayPal Official`插件很难安装成功, 也可以把插件直接解压到`module`文件夹下  
比如出现这个错误时: `An error occurred while installing PayPal Official.`
![pic](./202406-prestashop学习/0221.png)  
安装完毕
![pic](./202406-prestashop学习/023.png)  
---
## 配置界面:  
先安装启用插件
![pic](./202406-prestashop学习/027.png)  
然后到支付方式列表中配置
![pic](./202406-prestashop学习/023.png)
进入插件配置页面
![pic](./202406-prestashop学习/024.png)
点击`Logout/Switch Mode`配置账号信息
![pic](./202406-prestashop学习/025.png)
![pic](./202406-prestashop学习/029.png)
![pic](./202406-prestashop学习/026.png)

### 使用中国账号(测试环境无法成功link):
#### 测试环境:
```yml
email_address:ark@test.com
password:Qq111222333
```
![pic](./202406-prestashop学习/030.png)
![pic](./202406-prestashop学习/031.png)
![pic](./202406-prestashop学习/032.png)
#### 正式环境
`gpscntest@outlook.com`
![pic](./202406-prestashop学习/033.png)
![pic](./202406-prestashop学习/034.png)
link成功
![pic](./202406-prestashop学习/035.png)  
配置:
![pic](./202406-prestashop学习/036.png)  
设置详情: 
![pic](./202406-prestashop学习/037.png)  
vault无法被启用  
![pic](./202406-prestashop学习/038.png)  
启用状态:
![pic](./202406-prestashop学习/039.png) 
PayPal支付方式在各个页面的展示情况: 
![pic](./202406-prestashop学习/040.png) 
![pic](./202406-prestashop学习/041.png) 
![pic](./202406-prestashop学习/042.png) 
![pic](./202406-prestashop学习/043.png) 
![pic](./202406-prestashop学习/044.png) 
点击`place order`后的新标签页展示
![pic](./202406-prestashop学习/045.png) 

---
## 使用PrestaShop Checkout built with PayPal 插件
Disable `paypal official`插件
![pic](./202406-prestashop学习/046.png) 
进入到`PrestaShop Checkout built with PayPal`插件配置页中, 关联
![pic](./202406-prestashop学习/047.png) 
![pic](./202406-prestashop学习/048.png) 
在link PayPal账号前需要link Prestashop账号
![pic](./202406-prestashop学习/049.png) 
![pic](./202406-prestashop学习/050.png) 
Link完成, 可以link PayPal 账号
![pic](./202406-prestashop学习/051.png) 
Onboard:
![pic](./202406-prestashop学习/052.png) 
![pic](./202406-prestashop学习/053.png) 
![pic](./202406-prestashop学习/054.png) 
![pic](./202406-prestashop学习/055.png) 
在测试沙箱环境的中国账号的时候, 发现出现了和另外一个插件一样的错误:
![pic](./202406-prestashop学习/056.png) 