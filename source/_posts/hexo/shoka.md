---
title: shoka配置
categories:
  - [hexo]
tags: [主题配置]
---

#### 参考

- http://www.4k8k.xyz/article/BetrayVirginia/113572364
- https://shoka.lostyu.me/

#### Bug 处理

1. 在使用这个样式之后, 我发现我的代码块第一行总是开头空两格!!!,我以为是配置问题, 于是我就一直找这个问题作者的配置文件都快被我翻烂了。。。
   ![](https://tva1.sinaimg.cn/large/006xVoHRly8gzj0mjjaqlj30up04y3yn.jpg)
   很离谱的一个 bug.
   然后,我在写博客的时候,无意中想到检查一下元素。。。故事开始了
   ![](https://tva1.sinaimg.cn/large/006xVoHRly8gzj0q2bwslj30lk0iymz4.jpg)
   发现是伪类元素 before 搞得鬼,然后我就想，我直接把样式改了不就完事？
   所以就在
   ![](https://tva3.sinaimg.cn/large/006xVoHRly8gzj0rutbl9j308h0gxmy4.jpg)
   中发现了要该的样式
   ![](https://tva4.sinaimg.cn/large/006xVoHRly8gzj0swxshij308q0743yt.jpg)
   问题解决
