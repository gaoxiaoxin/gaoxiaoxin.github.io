---
title: Css使用小技巧
categories:
  - [Css]
tags: [小技巧]
---

这个后面会不定时更新

#### Css 特效

1. css 打字效果

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./base.css" />
    <style>
      .wrapper {
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        /* 只是实现居中功能 */
      }
      .typing-demo {
        width: 22ch; /* ch 长度为 ‘0’ */
        animation: typing 4s steps(20) infinite, blink 0.5s step-end infinite
            alternate;
        /* blink 使用 border-right 来模拟一个输入的竖线 */
        white-space: nowrap;
        overflow: hidden;
        /* 设置超出隐藏 */
        border-right: 3px solid;
        font-size: 2em;
      }

      @keyframes typing {
        0% {
          width: 0;
        }
        50% {
          width: 22ch;
        }
        100% {
          width: 22ch;
        }
      }
      @keyframes blink {
        50% {
          border-color: transparent;
        }
      }

      /* 
        animation : 
          1. animation-name
          2. animation-duration => 指定一个动画周期的时长
          3. animation-timing-function(缓动函数) =>
            1. 定义CSS动画在每一动画周期中执行的节奏
            2. 对于关键帧动画来说，timing function作用于一个关键帧周期而非整个动画周期，即从关键帧开始开始，到关键帧结束结束。
          4. animation-delay: 设置动画延迟时间
          5. animation-iteration-count: 设置动画的循环次数
          6. animation-direction: 设置动画在循环中是否反向运动
          7. animation-fill-mode: 设置在对象动画时间之外的状态
          8. animation-play-state: 设置对象动画的状态
      */
    </style>
  </head>
  <body>
    <div class="wrapper">
      <div class="typing-demo">有趣且实用的CSS小技巧</div>
    </div>
  </body>
</html>
```

#### 引用

1. <a href="https://mp.weixin.qq.com/s/pVcnuf4R2qEbJXDpS9E37A#">有趣且实用的 css 技巧</a>
