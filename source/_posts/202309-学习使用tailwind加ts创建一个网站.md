---
title: 202309-学习使用tailwind加ts创建一个网站
date: 2023-09-11 14:19:40
categories:
- 教程
tags: 
- js
- ts
- css
- tailwind
- PayPal
---

创建repo, 选私有库, 之后要放到PayPal内部Git上的, 先声明一下叠甲避免安全性问题
![pic](202309-学习使用tailwind加ts创建一个网站/001.png)

创建ts的create项目
```bash
npx create-react-app display-site --template typescript
```
![pic](202309-学习使用tailwind加ts创建一个网站/002.png)
发现无法push代码
![pic](202309-学习使用tailwind加ts创建一个网站/004.png)
想到可能是没有权限的问题, invite自己的GitHub账号
![pic](202309-学习使用tailwind加ts创建一个网站/003.png)
就可以成功了
![pic](202309-学习使用tailwind加ts创建一个网站/005.png)
删掉创建工程时的一些默认项
![pic](202309-学习使用tailwind加ts创建一个网站/006.png)
参考开发文档
![pic](202309-学习使用tailwind加ts创建一个网站/007.png)
安装tailwind
![pic](202309-学习使用tailwind加ts创建一个网站/008.png)
在主 CSS 文件中通过 @tailwind 指令加载 Tailwind 功能模块
![pic](202309-学习使用tailwind加ts创建一个网站/009.png)
添加tailwind的插件, 不然没有自动提示, 包括颜色什么的
![pic](202309-学习使用tailwind加ts创建一个网站/010.png)
发现没有效果
![pic](202309-学习使用tailwind加ts创建一个网站/011.png)
原因是官网的示例代码不正确, 可能是我参考的入口不对, 但是这里配置文件需要自己把 `ts,tsx` 添加上去
```json
 content: ["./src/**/*.{html,js,ts,tsx,jsx}",'./src/pages/**/*.{js,ts,jsx,tsx}', './src/components/**/*.{js,ts,jsx,tsx}']
```
![pic](202309-学习使用tailwind加ts创建一个网站/012.png)
有效果了
![pic](202309-学习使用tailwind加ts创建一个网站/013.png)

在搜索侧边栏的样式过程中, 我发现了一个文章, 可能这样安装`tailwind`更好
```bash
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```
> https://devpress.csdn.net/react/62ed61bbc6770329307f245c.html
> https://codepen.io/junchow/pen/GRoJPeo?editors=1000

---
为了使用next框架, 只能重建项目了, 因为next虽然是基于react的, 但是使用了SSR服务端渲染, 没有什么比较好的办法可以直接从react项目转过来, 所以重新建一个项目了.

![pic](202309-学习使用tailwind加ts创建一个网站/014.png)

---
nextjs的使用办法, 我这里就不说了, 毕竟很多, 看文档比较好  
但是在我使用 `create-next-app` 的这个时间节点 (2023-09-13), next版本已经从12变成了13, 这里有一个天坑的地方, nextjs是一个以规范为导向的框架, 也就是说你的文件名, 文件夹名字是要符合规范的, 比如在老的12以前的版本中, 路由不需要自己配置, 放在 `pages\文件夹名\页面名[url参数名].js`下就可以了, 这个`文件夹名\页面名` 就是最终的访问路径.   
但是这么关键的东西, 在`13`版本里改了但是文档里说的却不清楚  
这就很要命了, 现在的结构是, 每一个页面, 都是一套组合拳, 比如`test`这个页面, 会有自己的`layout` 和 `page`, 并且命名规范变成了文件夹导向, 文件夹下的默认文件也从 `index.jsx` 变成了 `page.tsx`, 这谁扛得住啊? 看看现在的目录是这样的
![pic](202309-学习使用tailwind加ts创建一个网站/015.png)  

原来的`pages` 变成了现在的 `app`, 而且官方文档还没说! 而且这个功能还必须要修改配置文件, 不然还不会生效, 
![pic](202309-学习使用tailwind加ts创建一个网站/016.png) 

但是令人困惑的是, 既然写了是`experimental`功能, 那在使用`cli`的时候让人选一下是不是要启用这点吧. 它也没有, 默认都是有的.

---
反正, 就这么先用着吧, 毕竟next也算是react下一个很火的框架

然后是第一次使用了`heroicons`, 这个库的确很方便, 直接把icon变成了`ReactNode`类型, 直接用就可以, 但是里面的图标还不是很多, 不比直接从svg比如iconfont上多, 但是svg还需要转一下, 我现在也没啥好的转的方法找到.

---
一个关于nextjs 13比较好的帖子
> https://juejin.cn/post/7247740446280007741

总结几个比较坑的地方:
1. 在debug和打包的时候, 临时文件/部署文件默认是放在`.next`文件夹下的, 如果要修改为熟悉的名字比如`dist`,`build`, 需要添加配置项
![pic](202309-学习使用tailwind加ts创建一个网站/017.png) 

2. 因为nextjs服务器端渲染的特性, 所以它打包出来的部署文件不是静态的html文件, 没有`index.html`, 这点不用特别大惊小怪, 如果想要变成静态文件, 也需要添加配置项
![pic](202309-学习使用tailwind加ts创建一个网站/018.png) 

3. 配置一些其他的简单配置项
> https://www.cnblogs.com/blogs-xlf/p/14014656.html

4. 一个新的电子邮箱账号
```yml
addr: browser.sync.cityofwind@gmail.com
pwd: Qq111222333
```

5. 如何在render中部署一个nextjs项目
> https://render.com/docs/deploy-nextjs-app

6. next比较好的非官方教程
> https://www.builder.io/blog/next-13-app-router
> https://juejin.cn/post/7247044423024721980

---
这里有一个坑爹的地方, 因为我做的仅仅是一个类似demo的网站, 所以很多按钮是按了没有效果的, 就想用popover的这个方式来做一下提示, 之前我尝试过 `tippy.js` 这里本来也是想用的. 但是其在`typescript`中那是完全没法用啊(这个库有点问题, 在强类型下, 有2个关键参数没有类型, 使用者又不知道应该是什么), 放弃 `tippy.js`了, 在兜兜转转了好久之后, 放弃了转而使用`material UI`
> https://mui.com/material-ui/react-tooltip/

![pic](202309-学习使用tailwind加ts创建一个网站/019.png) 
```bash
yarn add @mui/material @emotion/react @emotion/styled @heroicons/react  react-router-dom
```

不知道为什么这么创建出来的react中ts的支持是要手动加上去的
```
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```
然后把src/index.js 重命名为 src/index.tsx


