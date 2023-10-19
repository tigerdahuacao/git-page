---
title: 202310-关于tailwind的一些碎碎念
date: 2023-10-10 21:38:56
categories:
- 开发随笔
tags: 
- tailwind
---

使用`tailwind`有一段时间, 写一些自己的小感触

1. 这个class类使用@apply的复用就没啥用, 还不如把变成字符串常量, 使用这个方式`className={DIV_CLASS}`来进行复用
> https://tailwindcss.com/docs/reusing-styles#extracting-classes-with-apply

2. 如果不是tailwind中自定义的类的话, 可以这么用来进行css的管理
> https://juejin.cn/post/7031528538209681444
事实上这样用的确是最好的, 有个大佬也推荐不要使用类似`tailwind`这样的css框架, 因为会导致代码混乱
可以把`tailwind`中的class实际背后的css摘出来(使用vscode可以提示出这一点), 使用原来的css

3. 或者是使用 `classNames` 这个包来进行css的式样管理, 特别是在需要进行式样切换的地方, 我用了一下感觉还挺好的
> https://github.com/JedWatson/classnames

4. `tailwind`的组件是要收费的, 这很恶心, CSDN上似乎有组件的源码可以下载导入使用, 我还没试过. 还是老老实实看tw的文档查看一下用法来的实在

5. tailwind是属于postCSS的一种, 也就是说, 它会对css代码进行事先的编译优化, 比如像tailwind自己宣传的那样, 删除不必要的代码
> https://juejin.cn/post/7200782261997338681
所以这些postCSS的东西必不可少
```bash
pnpm i -D tailwindcss postcss autoprefixer
# yarn add -D tailwindcss postcss autoprefixer
# npm i -D tailwindcss postcss autoprefixer
```