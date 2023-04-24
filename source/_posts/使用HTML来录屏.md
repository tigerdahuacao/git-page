---
title: 使用HTML来录屏
date: 2022-12-05 10:58:41
categories:
- 小工具
tags: 
- html
---

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <video class="video" width="600px" controls></video>
        <button class="record-btn">Click Me</button>
        <script>
            let btn = document.querySelector(".record-btn");
            btn.addEventListener("click", async () => {
                //询问,你要录哪个对象
                let stream = await navigator.mediaDevices.getDisplayMedia({
                    video: true,
                });

                //better browser support
                const mime = MediaRecorder.isTypeSupported(
                    "video/webm; codecs=vp9"
                )
                    ? "video/webm; codecs=vp9"
                    : "video/webm;";

                // 录制流对象
                let mediaRecorder = new MediaRecorder(stream, {
                    mimeType: mime,
                });

                let chunks = [];
                mediaRecorder.addEventListener("dataavailable", function (e) {
                    chunks.push(e.data);
                });

                mediaRecorder.addEventListener("stop", () => {
                    let blob = new Blob(chunks, {
                        type: chunks[0].type,
                    });

                    let url = URL.createObjectURL(blob);
                    let video = document.querySelector("video");
                    video.src = url;

                    let a = document.createElement("a");
                    a.href = url;
                    a.download = "video.webm";
                    a.click();
                });

                mediaRecorder.start();
            });
        </script>
    </body>
</html>
```