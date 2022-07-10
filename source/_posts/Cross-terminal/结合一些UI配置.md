---
title: "结合一些UI配置"
sticky: false
categories:
  - ["跨端"]
tag: ["uniapp", "uView"]
---

#### 前言

光修炼也不行, 那么我们也需要练一些招式(UI), 正常人都去自己练, 但我们不一样,我们是挂逼,我们直接去外挂(百度)的商城找我们想要的招式。 经过挑选, 我们发现`uView`是一个强大的招式(UI 组件库),那么挂壁如我们,我们就直接购买然后消化

#### 购买(下载)

https://www.uviewui.com/components/intro.html, 那么我们可以去官网看看.
这里因为我们是基于 vscode 去写 uniapp.那么,我们就直接选择 npm 的安装方式.

```xshell demo
npm install uview-ui
```

![](https://tva2.sinaimg.cn/large/006xVoHRly8h1cln71lb8j30rn09mq3x.jpg)

https://www.uviewui.com/components/npmSetting.html (最好去看官网，这里可能因为时间原因会更改)
我们这里需要下载一些`uView`的配件,因为这个组件库其实也是基于 vue2 写的, 就是我们平时的 UI 库,只不过封装的更好了而已(大佬们牛)

```xshell demo
// 安装sass
npm i sass -D
// 安装sass-loader 翻译官(主要是基于webpack)
npm i sass-loader@10 -D
```

这里如果对 webpack 不太理解,可以忽略, 或者去看我之前写的文章。

我们还需要配置一些东西:

1. 引入 uView 的 js 库
   在项目的入口(main.js, 也可以叫 app.js)处, 引用 uView 的 js 库.(Webpack 的依赖引用)

```js demo
import uView from "uview-ui";
Vue.use(uView);
```

2. 引入 uView 的全局 scss 文件
   在项目的`src`中的`uni.scss`中引入此文件

```scss demo
/* uni.scss */
@import "uview-ui/theme.scss";
```

3. 引入 uView 基础样式

```html demo
<style lang="scss">
  /* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
  @import "uview-ui/index.scss";
</style>
```

这里有一些小问题。

当我们敲出 `import` 的时候,会出现
![](https://tva2.sinaimg.cn/large/006xVoHRly8h1cmlruk38j30d60233yg.jpg)
会出现两个不同的选项, 那么我们该如何区分

首先 `@import url()` 这个是 css 的一个特性, 为了开发者能将 css 分解成更小的部分, 它的引入会直接发起一个 Http 的请求.
而 `@import` 这个是 scss 的一个特性, 为了更加想我们使用的模块化. 它会直接将代码包含进目标 scss 文件. 那么现在来看,我们明白了。

4. 配置 easycom 组件模式
   这个需要在项目的`src`目录的`pages.json`中进行。

:::primary

1. uni-app 为了调试性能的原因，修改 easycom 规则不会实时生效，配置完后，您需要重启 HX 或者重新编译项目才能正常使用 uView 的功能。
2. 请确保您的 pages.json 中只有一个 easycom 字段，否则请自行合并多个引入规则。

:::

什么, 为什么我要配这个??
https://uniapp.dcloud.io/collocation/pages.html#easycom

5. Cli 模式额外配置

```js
// vue.config.js，如没有此文件则手动创建
module.exports = {
  transpileDependencies: ["uview-ui"],
};
```

什么什么又要弄我不懂的东西, 来看看吧
![](https://tva1.sinaimg.cn/large/006xVoHRly8h1cq1n6dv0j30n707ngmw.jpg)

我们因为在 node_modules 中的依赖太多, 我们一般在配置 webpack 的时候就会直接把它整个忽略, 而不去优化它, 但是现在明显它运用了 vue 单文件, 我们如果不进行转换那么我们就真的无法使用,bbq。所以要配置

然后用就完了。
