---
title: 本地使用wordpress创建wooCommerce
date: 2023-02-02 10:51:23
categories:
- 开发随笔
- 教程
tags: 
- 部署
---

首先要明确, wooCommerce是开源而免费的, 因为毕竟它是基于wordpress的, 但是如果你打开wooCommerce的官网, 基本上就会发现是要收费的, 这点很奇怪.
![pages](本地使用wordpress创建wooCommerce/102.png)
可以看见这幅图里, 它是要你用wordpress账号进行授权登录, 可以wordpress就不应该会要收费
![pages](本地使用wordpress创建wooCommerce/103.png)

后来我明白了, 应该是wooCommerce官网, 它为了方便提供了基于云服务器服务的傻瓜式操作, 而这个服务器主机总归是要花钱的, 但是wooCommerce本身是不要花钱的. 如果你使用自己的服务器, 那就不用花钱了.

之前学习云主机的时候没好好学, 直接用了wordpress的镜像, 那现在要做的就是在本地配置wordpress服务, 并且安装wooCommerce插件.

wordpress是基于php的, 所以在windows上安装phpstudy(用其他的类似xampp也可以, 但是经过php程序员指点, 还是用这个比较好). 如果是在linux上, 就是用bt. 

然后把解压后的wordpress放到WWW目录下. 启动web Server(apache和nginx随便哪个都行)和db server.
![pages](本地使用wordpress创建wooCommerce/104.png)

启动你wordpress的路径, 会出现让你第一次配置的图(如果没有找到` wp-config.php `这个配置文件的话)
![pages](本地使用wordpress创建wooCommerce/105.jpg)
可以发现这里要你填写数据库名, 而这里就比较蛋疼了.

如果你的电脑上没有mysql, 那只有一个mysql服务, 使用的端口是3306, 而如果你的电脑上已经按照了mysql, 那phpstudy想要启动的mysqld服务, 默认端口也是3306, 就会出问题, 而且后面启动网页的数据库链接也是都是默认3306, 会链接到不受phpstudy管理数据库的, 所以要添加一下配置文件. 
1. 修改phpstudy的mysql端口号:
![pages](本地使用wordpress创建wooCommerce/004.png)
修改port为3308(或者其他什么的)
![pages](本地使用wordpress创建wooCommerce/0011.png)
2. 给phpMyAdmin修改端口号, 新建` config.inc.php `文件(可以从原来就有的` config.sample.inc.php `文件中复制过滤)
![pages](本地使用wordpress创建wooCommerce/001.png)
![pages](本地使用wordpress创建wooCommerce/002.png)
可以看到修改为3308也就是和mysql服务的3306错开, 就可以链接上了
![pages](本地使用wordpress创建wooCommerce/003.png)
3. 此时虽然phpstudy的链接和启动好了, 但是wordpress的默认数据库链接还没有改, 此时再进入wordpress会进入这个页面
![pages](本地使用wordpress创建wooCommerce/005.png)
这就要求手动配置wordpress的配置文件` wp-config.php `
![pages](本地使用wordpress创建wooCommerce/006.png)
和phpMyAdmin的配置文件一样, 这个配置文件也是要自己新建的

接下来就是安装wooCommerce, 可以在wordpress的插件市场中安装, 但是那个安装速度很慢还容易出错, 建议在wordpress plugin的网页版中搜索插件, 然后再下载安装.
![pages](本地使用wordpress创建wooCommerce/2001.jpg)
![pages](本地使用wordpress创建wooCommerce/2002.jpg)
![pages](本地使用wordpress创建wooCommerce/2003.png)
下载好之后, 可以在UI界面中, 把zip文件上传并安装, 但是我试了试, 这一步很慢会导致出错安装失败
![pages](本地使用wordpress创建wooCommerce/2004.jpg)
所以还是建议解压之后, 安装到plugin目录下
![pages](本地使用wordpress创建wooCommerce/2005.jpg)
插件就会被识别并出现了
![pages](本地使用wordpress创建wooCommerce/2006.jpg)
启动wooCommerce插件后, 会进入它的引到页面  
然后就是一直下一步就可以, 最近建一些测试商品
![pages](本地使用wordpress创建wooCommerce/2007.jpg)
![pages](本地使用wordpress创建wooCommerce/007.png)

---
最后就是设置付款收款的问题, 因为wordpress默认的收款方式, 是要和wordpress的账号进行关联的, 这一步又容易网络不好而导致不成功, 可以直接安装Paypal的插件, 安装方法同理, 解压放到插件目录下.  
1. 填写对应信息
![pages](本地使用wordpress创建wooCommerce/008.png)
![pages](本地使用wordpress创建wooCommerce/010.png)
![pages](本地使用wordpress创建wooCommerce/011.png)
2. 设置完成
![pages](本地使用wordpress创建wooCommerce/012.png)
3. 在页面中就可以使用Paypal进行收款了
![pages](本地使用wordpress创建wooCommerce/013.png)

至此你的本地wooCommerce就可以搭建完毕了, 如果是为了真实运营, 那么把它部署到公网服务器上就可以了. 另外一点就是, 默认的主题一片白的也没有交互(毕竟wordpress是个博客工具, 要变成电子商城肯定要魔改), 有很多针对wooCommerce开发的购物车风格的主题比如flatsome之类, 可以去试用或者在本地自己刷刷, 但是这些主题都是收费的, 真实运营的话, 不购买后使用可能会收律师函
