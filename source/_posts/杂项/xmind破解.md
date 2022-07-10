---
title: xmind免费使用与安装
sticky: false
math: true
categories:
  - [杂项]
---

#### 前提

博主的环境为 win10 环境

#### 参考

- https://www.jianshu.com/p/3e511eb25f66

:::primary
和上面这个博主的基本一样，只是加了一些 win10 的错误解决
:::

#### 安装

一. 下载 XMind

Win: http://dl2.xmind.cn/xmind-8-update4-windows.exe
Mac: http://dl2.xmind.cn/xmind-8-update4-macosx.dmg

然后一直安装就行, 确定对应的解压路径

二. 下载补丁
链接: https://pan.baidu.com/s/1NgdssmZHdEe_TTas6OHKtQ 提取码: xuqa 复制这段内容后打开百度网盘手机 App，操作更方便哦
--来自百度网盘超级会员 v3 的分享

三.将补丁复制到安装路径的根目录(刚才解压的路径)
![](https://tva3.sinaimg.cn/large/006xVoHRly8h0zu9xk23xj30wo0in776.jpg)

四.找到安装目录中的 Xmind.ini

在最后一行添加代码
![](https://tva1.sinaimg.cn/large/006xVoHRly8h0zubgzni1j30lq0osgnk.jpg)

五. 找到文件 hosts，在末尾添加 (文件在 C:\Windows\System32\drivers\etc 目录下，不是：hosts.ics)

!!!! 这里 win10 不同
非 win10:
127.0.0.1 xmind.net
127.0.0.1 www.xmind.net
win10

```shell demo
# 127.0.0.1 xmind.net
# 127.0.0.1 www.xmind.net
```

六. 打开 Xmind->帮助->序列号->输入序列号【邮箱：随意输】

序列号在 readme.txt 中

注意: 需要按照步骤来，不按步骤来，会出现

验证失败，可能为 XMind 2013 Pro 序列号。XMind 8 pro 激活教程
