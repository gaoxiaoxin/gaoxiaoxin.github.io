---
title: 使用vscode写uniapp
sticky: true
categories:
  - ["跨端"]
tag: ["uniapp"]
---

#### 前言

今日再开一领域, 虽然之前也写了两个 uniapp 的项目,但是并没有进行总结什么的, 然后就忘掉了,bbq. 无所谓, 现在也不迟。
:::primary
要是有所得罪请原谅, 本是出自一番好意,
只是想显点粗浅技艺, 那才是我们的初衷。
-- 威廉·莎士比亚，《仲夏夜之梦》
:::

#### 开始安装

之前我们都比较认可的一件事是写 uniapp 使用 HBuilderX 去使用会最好, 毕竟这是人家 uniapp 官网都推荐的一个编辑器(国产), 确实使用效果也不错基本就是切合`uniapp`的各种功能而生的,但是，对于一个用 vscode 开发了 2 年左右的前端来说
它真的是不符合我的使用习惯(我舍不得 vscode 上的插件)
那么我们又去想, 其实 vscode 的最强大的功能是什么? 就是它的可扩展性, 那必须**安排**

#### 推荐文章

先把我看的教程文章放在上面，以示尊敬!!!(ps: 主要很~权威~棒) 对照下面两个文章食用更香哦

- https://ask.dcloud.net.cn/article/36286
- https://blog.csdn.net/weixin_44670973/article/details/109221196

我在这些大佬的基础上, 添加了我遇到的问题以及解决方案

#### 开发环境搭建

1. 基于 vue, 必须先在全局安装 vue-cli 3.x 脚手架

```xshell demo
npm install -g @vue/cli
```

2. 通过 CLI 创建 uni-app 项目

```xshell createDemo
vue create -p dcloudio/uni-preset-vue my-project
```

这个时候会提示创建项目, 我建议大家选择`默认模板`, 在上面的教程中, 推荐使用 Hello uni-app 模板, 但是这样的话，就会在我们的项目目录那一块多很多东西, 新手容易直接懵掉。

那么当我们创建项目的同时，我们可以去安装一些插件去兼容我们的 uniapp ,

3. 安装插件 以及 依赖

- 虽然在上面的文章中也指出去安装 vetur, 但是，我想说但是啊, 我看文章的日期是 2019 年, 那个时候还没有 vue3, 尤大大也没推荐 `Volar` 插件, 所以，我只能说，下载 `veter` 这个插件的目的就只是要 vue 的代码提示, `volar`也可以实现。大家可以自己选择去使用。
  ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64c65748853845df91ffaba7205ae870~tplv-k3u1fbpfcp-zoom-crop-mark:1304:1304:1304:734.awebp)
- 上面的文章有说, CLI 工程默认带了 uni-app 语法提示和 5+App 的语法提示.但是不知道是不是我脸黑，我这就没有。但是这难不倒我, vscode 大法好, 我们直接去找插件, 在插件搜索那一块, 我们就直接
  搜索`uniapp`然后找到 `uniapp-snippet`, 下载就行。同样可以实现代码提示
- 安装组件语法提示 (如果不成功就重新启动一下 vscode)

```xshell demo
npm i @dcloudio/uni-helper-json
```

- 导入 HBuilderX 自带的代码块
  从 <a href="https://github.com/zhetengbiji/uniapp-snippets-vscode">Github</a>中下载,
  <a href="https://gitee.com/lingyun-my/vscode-snippets-uniapp">git</a>中下载，放在项目目录下的.vscode 目录中
  ps: 如果完成后没有作用就重新启动一下 vscode

#### 运行项目

这个 在 `package.json`中已经写好了很多的脚本命令，对应很多的项目运行方式, `npm run` 就行

#### 结尾

![](https://tva1.sinaimg.cn/large/006xVoHRly8h1bwf1b9ahj31400u0dod.jpg)
生活不只有代码
