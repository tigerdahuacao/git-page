---
title: 使用nodejs把文本文件的行颠倒
date: 2023-05-11 14:54:49
categories:
- 小工具
tags: 
- nodejs
---


自己写的代码, 用于把一个文本文件的最后一行变成第一行,最后第二行变成第二行以此类推,
主要是看这个贴子里有这个需要:

> https://zhuanlan.zhihu.com/p/351494670

```javascript
const { createReadStream, createWriteStream } = require("fs");
const { createInterface } = require("readline");

const rl = createInterface({
    input: createReadStream("requirement.txt"),
    output: createWriteStream("requirement-new.txt"),
    terminal: false,
});

rl.write("hello world")

const lines = [];
// rl.on('line', (line) => {
//     // lines.push(line)
//     rl.write(line)
// });

rl.on("line", (line) => {
    lines.push(line);

    console.log(line)
    rl.write(line);
})
// .on("close", () => {
//     lines.reverse();
//     console.log(lines);
// });

```