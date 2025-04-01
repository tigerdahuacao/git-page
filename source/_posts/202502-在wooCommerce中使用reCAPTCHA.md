---
title: 202502-在wooCommerce中使用reCAPTCHA
date: 2025-02-18 16:37:25

categories:
- 开发随笔
tags: 
- wooCommerce
---

## 1.使用Google reCAPTCHA
插件安装
![pic](./202502-在wooCommerce中使用reCAPTCHA/001.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/002.png)

插件安装前的效果
![pic](./202502-在wooCommerce中使用reCAPTCHA/003.png)


`https://www.google.com/recaptcha/admin/create`
![pic](./202502-在wooCommerce中使用reCAPTCHA/004.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/005.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/006.png)

选择你需要加载`reCAPTCHA`的页面, 并`保存更改`
![pic](./202502-在wooCommerce中使用reCAPTCHA/007.png)

在第一次保存后, 会出现一个这个测试用的框, 来测试你的配置是否正确
![pic](./202502-在wooCommerce中使用reCAPTCHA/008.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/009.png)
注意密钥的顺序
![pic](./202502-在wooCommerce中使用reCAPTCHA/010.png)
出现这样的错误, 可以调整验证策略
![pic](./202502-在wooCommerce中使用reCAPTCHA/011.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/012.png)

插件安装后的效果
![pic](./202502-在wooCommerce中使用reCAPTCHA/014.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/015.png)

但是事实上, 我发现这个验证过程存在bug, 即:就算buyer完成了人机验证, 它时不时依然会报验证失败的错误. 十分annoying

---

## 2. 使用cloudFlare turnstile

插件安装
![pic](./202502-在wooCommerce中使用reCAPTCHA/016.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/017.png)

根据指示, 到`cloudflare`上完成注册
![pic](./202502-在wooCommerce中使用reCAPTCHA/019.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/020.png)
注册完成
![pic](./202502-在wooCommerce中使用reCAPTCHA/021.png)

配置插件
![pic](./202502-在wooCommerce中使用reCAPTCHA/022.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/023.png)
配置完插件后, 它也要验证配置信息是否正确
![pic](./202502-在wooCommerce中使用reCAPTCHA/024.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/025.png)

插件的展示效果
![pic](./202502-在wooCommerce中使用reCAPTCHA/028.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/026.png)
![pic](./202502-在wooCommerce中使用reCAPTCHA/027.png)

可以去官网找到测试组件, 完成一些自定义测试, 但是因为这个插件需要verify有效性, 所以没有办法做NG测试  
`https://developers.cloudflare.com/turnstile/tutorials/excluding-turnstile-from-e2e-tests/`  
`https://developers.cloudflare.com/turnstile/troubleshooting/testing/`
![pic](./202502-在wooCommerce中使用reCAPTCHA/029.png)



