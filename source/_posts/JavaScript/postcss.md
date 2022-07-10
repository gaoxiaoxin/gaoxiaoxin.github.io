---
title: postcss的使用
sticky: true
# categories:
#   - [JavaScript]
# tags:
---

先上官网 https://postcss.org/ ,
这个 logo 好看的很![](https://camo.githubusercontent.com/a2ebaaedf9af41416a2717b3a28f405b39535397f4463c5c5119146c84c240f9/68747470733a2f2f706f73746373732e6f72672f6c6f676f2e737667)

#### 什么是 PostCss

PostCSS 是一个允许使用 JS 插件转换样式的工具。 这些插件可以检查（lint）你的 CSS，支持 CSS Variables 和 Mixins， 编译尚未被浏览器广泛支持的先进的 CSS 语法，内联图片，以及其它很多优秀的功能。

#### 中间待补充部分

#### 在 webpack 中使用 postcss

在 webpack 中我们可以使用 postcss 进行 css 的兼容性处理
其实 postcss 是一个单独的工具, 我们可以使用它单独的去转换 css, 首先需要下载 postcss postcss-cli

```js npm
npm install postcss postcss-cli -D
```

其中 postcss 是工具本身， postcss-cli 是需要运行的环境。
我们转换 css 的时候要用到对应的插件, 这里我们使用 autoprefixer 来进行处理

```js npm
npm install autoprefixer -D
```

然后在命令行中

```js npm
npx postcss --use 要使用的插件 -o 要处理的文件地址 要输出的地址
npx postcss --use autoprefixer -o end.css ./src/css/style.css
```

我们使用 postcss-loader 对 css 文件进行处理, 所以需要先下载 postcss-loader 插件

```js npm
npm install postcss-loader -D
```

然后去配置文件中进行配置

```js webpack.config.js
{
  loader: "postcss-loader",
  options: {
    postcssOptions: {
      plugins: [
        // postcss的插件
        require("autoprefixer"),
      ],
      }
    },
},
```

或者可以将 postcss 配置独立出来到 postcss.config.js 文件中

```js postcss.config.js
module.exports = {
  plugins: [
    // 一般不使用这个
    // require('autoprefixer')
    // 一般用这个
    require("postcss-preset-env"),
  ],
};
```

这里我们用了另外的一个插件 postcss-preset-env 这个插件内置 autoprefixer, 并且比 autoprefixer 好用很多
