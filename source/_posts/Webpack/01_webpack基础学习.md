---
title: Webpack学习
categories: -[Webpack]
tags: [webpack5]
---

#### 写作的原因

最近发现一个事, 发现自己学是学了, 但是学完之后没有对自己学习的内容进行一个总结, 然后就发现自己学了但是没有完全学会。。。, 所以还是得总结和思考!!!
:::primary
学而不思则罔,思而不学则殆
:::

#### 前言

我们学习前端, 先是 Html,Css,Js,自己手动的写一些引入链接,慢慢的我们了解到 less, sass/scss,等等这些语言, 但是我们不能直接在我们的 Html 中直接引用这些东西, 那么我们就需要使用各种工具先将他们转化为 css,然后再去使用。很麻烦,但也无可奈何。 慢慢的我们去学习各种框架, 并且了解到他们的 cli,并且他们可以直接使用 less, scss 这些东西，只是配置一些配置文件。那么 why?
这是因为他们在内部使用了一些打包工具, 这里用 vue-cli 举例, 它内置的是 webpack,使用 webpack 进行打包。so, webpack 是什么呢？

+++info 什么是 webpack
webpack 是一个用于现代 JavaScript 应用程序的 静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。
+++

#### Webpack 基本使用

##### 环境安装

当我们新创建一个项目, 那我们就需要下载 webpack 以及运行 webpack 的环境 webpack-cli, 前置条件是需要 node 环境.

```js npm
npm init -y // 项目起一个英文名
npm install webpack webpack-cli -d
```

哈哈，写完上面代码块, 解决了博客的一个 bug,舒服。
然后我们整理一下我们的项目结构, 我们需要新建一个`src`文件夹,在其中放我们的代码。
然后在`src`的外层创建一个 webpack.config.js 的 webpack 的配置文件

##### webpack 的一些核心概念

- `entry` 入口, 指示 webpack 应该使用那个模块作为开始。
- `output` 出口, 将打包好的文件放在那个地方
- `loader` “翻译官” 去转换和处理 webpack 不能理解的文件, webpack 默认只理解==JavaScript==和==Json==。
- `plugin` 插件, 做一些 loader 做不了的事情, 执行更加广泛的任务。如打包优化, 资源管理, 环境变量注入等等。
- `mode` 模式, 通过设置不同的模式, 来启动 wwebpack 内置在相应环境下的优化。

##### webpack 的基本使用

:::warning
对文件的处理都是必须在入口的依赖中
:::

###### 基础的打包

我们先使用最基础的 webpack 对 js, 进行一个基础的打包, 慢慢的引入。

```js index.js
// ./src/index.js
const sum = (x, y) => {
  return x + y;
};
console.log(sum(20, 30));
```

然后编写 webpack.config.js 文件

```js webpack.config.js
const path = require("path"); // node工具

module.exports = {
  entry: "./src/index.js", // 入口文件
  output: {
    path: path.resolve(__dirname, "./build"), // 导出的文件夹地址
    filename: "bundle.js", // 文件名
  },
  mode: "development",
};
```

然后在命令行中运行 webpack 后, 我们就能看见对应 build 文件夹了
但是我们写前端项目往往还是需要一个 HTML 文件, 那么我们还是要像以前一样, 自动创建一个 HTML 文件, 然后自己去引入吗？答案当然是否

###### 自动创建 HTML 文件并且引入打包文件

这里的话我们就需要使用一个 webpack plugin ==> html-webpack-plugin
https://webpack.docschina.org/plugins/html-webpack-plugin/

```js npm
npm i html-webpack-plugin -d
```

然后在配置文件中引入并使用

```js webpack
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js",
  },
  plugins: [new HtmlWebpackPlugin()], // plugins中放的是一个个的plugin实例
  mode: "development",
};
```

运行 webpack 后,我们就会发现会有一个默认的 HTML 文件出现在打包后的代码中。
但是这个默认的文件看起来有些死板, 我们无法对其进行控制, so 我们有什么办法吗？有的,我们可以在它的插件配置中放入一个模板
首先: 我们在`src`文件夹里创建一个 `index.html` 文件。让其作为模板在其中写好咱们的默认配置。
然后在配置文件中进行配置。

```js webpack
plugins: [
  new HtmlWebpackPlugin({
    // 复制 './src/index.html'文件, 并自动引入打包输出的所有资源
    template: "./src/index.html",
  }),
];
```

然后运行 webpack。
我们现在解决了 html, 那么就要考虑一下 前端不可或缺的 css 了

###### 处理 样式(css, less, scss) 文件

1. 处理 css 文件
   我们这里使用 loader 去处理 css 文件, first: 下载, 就像一句话说的: 无图言叼
   你都没有下载, 你凭什么用！

```js npm
npm install style-loader css-loader -d
```

和 plugin 不同的是,loader 可以不用引用, 直接使用

写一个 css 文件, 并且在 index.js 中对其进行引用。
写配置文件

```js webpack
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  entry: "./src/index.js",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "build"),
  },
  module: {
    rules: [
      {
        /* 匹配对应文件 */
        test: /.\css$/,
        use: [
          // 执行顺序, 从后到前
          // 创建style标签, 将js中的样式资源进行插入, 添加到header中生效
          "style-loader",
          // 将css文件变为commonjs模块加载js中， 里面内容是样式字符串
          "css-loader",
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      // 复制 './src/index.html'文件, 并自动引入打包输出的所有资源
      template: "./src/index.html",
    }),
  ],
  mode: "development",
};
```

这里需要注意 loader 的执行顺序

2. 处理 less 或 scss 文件
   我们这里使用 less 进行举例。
   我们之前使用 less 的时候是先将 less 变为 css 然后再去使用, 现在也是一样, 不过之前是使用工具或者插件, 现在是使用 loader ==> less-loader。

first: install

```js npm
npm install less-loader -d
```

配置文件修改为:

```js webpack
module: {
    rules: [
      {
        /* 匹配对应文件 */
        test: /.\css$/,
        use: [
          // 执行顺序, 从后到前
          // 创建style标签, 将js中的样式资源进行插入, 添加到header中生效
          "style-loader",
          // 将css文件变为commonjs模块加载js中， 里面内容是样式字符串
          "css-loader",
        ],
      },
      {
        test: /.\less$/,
        use :[ "styel-loader", "css-loader", "less-loader"]
      }
    ],
  },
```

就像上面的配置文件上一样, 我们对其进行打包, 那么我们就会发现打包之后，我们是看不到 css 的代码的, 这就很不习惯, 也不方便检查。

3. 将 css 单独进行抽离
   这个看上去就和"翻译官" loader 无关, 所以我们使用 plugin ==> `mini-css-extract-plugin`

```js npm
npm install mini-css-extract-plugin -d
```

配置文件来看

```js webpack
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const miniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  entry: "./src/js/index.js",
  mode: "development",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "build"),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        // 使用插件的loader代替style-loader,可以上去看看style-loader的作用
        // 将js中的css抽到一个css文件中
        use: [miniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    // 这里如果不配置filename的话就会创建一个name是hash值的css文件
    new miniCssExtractPlugin({
      filename: "css/[name].css",
    }),
  ],
};
```

4. css 的兼容
   这个是使用 postcss 和它的 loader
   具体文章参考 https://gaoxiaoxin.github.io/2022/02/18/JavaScript/postcss/

5. css 的压缩
   需要 optimize-css-assets-webpack-plugin 插件

```js npm
npm install optimize-css-assets-webpack-plugin -d
```

然后引入使用就可

```js webpack
plugins: [
  new HtmlWebpackPlugin({
    template: "./src/index.html",
  }),
  new miniCssExtractPlugin({
    filename: "css/main.css",
  }),
  new OptimizeCssAssetsWebpackPlugin(),
];
```

###### 处理其他资源

在 webpack5 之前,我们可以使用 file-loader 处理其他的资源, 用 url-loader 来处理图片, 那么在 webpack5 中, 我们使用内置的资源模块类型（asset module type）
https://webpack.docschina.org/guides/asset-modules/
+++success 资源模块类型（asset module type）

- asset/resource 发送一个单独的文件并导出 URL。之前通过使用 file-loader 实现；
- asset/inline 导出一个资源的 data URI。之前通过使用 url-loader 实现；
- asset/source 导出资源的源代码。之前通过使用 raw-loader 实现；
- asset 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 url-loader，并且配置资源体积限制实现

+++

1. 处理图片资源
   直接看配置文件

```js webpack
{
  test: /\.(jpe?g|png|gif|svg)$/,
  type: "asset", //要使用的类型
  generator: {
    //输出的文件名
    filename: "img/[name]_[hash:6][ext]"
  },
  parser: {
    dataUrlCondition: {
      maxSize: 100 * 1024
    }
  }
},
```

2. 处理字体文件

```js webpack
{
  test: /\.(eot|ttf|woff2?)$/,
  type: "asset/resource",
  generator: {
    filename: "font/[name]_[hash:6][ext]"
  }
}
```

##### Babel 的使用

首先, 我们要清楚 babel 是一个什么东西？？？
:::info
babel 是一个工具链, 主要用于旧浏览器或者环境中将 ECMAScript 2015+代码转换为向后兼容版本的
JavaScript
:::
所以我们为什么要去使用它？
先进行下面操作

```js npm
npm install -g es-checker
es-checker
```

就会出现下面的情况, 大家可以看出来
![](https://tva4.sinaimg.cn/large/006xVoHRly8gzm3oy2359j30nq04qdgc.jpg)
这是什么东西呢？这个其实是我们电脑环境对 es6 的特性的支持
我们可以看到虽然一片大绿，但是还是有一些小红, 说明我们电脑对 es6 还是不能全面支持(别人的说不定连你的也不如, 并且我们不知道用户电脑对 es6 的支持到底支持到什么地步), 所以对于一些代码我们需要写对应的 `ployfill`(补丁), 但是我们总不能全部自己写一遍吧!!!
:::danger
或者解决不了问题, 就解决提出问题的人
:::
哈哈, 开玩笑我们的用户可是上帝, 所以我们就要用上面提到的 babel 去将我们的 es6 代码全面转换到 es5, 解决支持问题。

so, 如何使用是个问题

###### babel 单独使用

这里先搁浅, 后面单独出一个 babel 的使用

###### 在 webpack 中的具体使用

使用 babel-loader, 这里需要安装一些东西, `@babel/core`和`babel-loader`,既然是 loader 那么我们就直接开始使用:

```js webpack
{
  test: /\.js$/,
  loader: 'babel-loader'
},
```

这样配置后, 我们可以在 index.js 中写一些 es6 的代码

```js index.js
const message = "Hello World";
const names = ["abc", "cba", "nba"];
names.forEach((item) => console.log(item));
```

然后运行进行打包, 但是我们却会发现它并没有按我们的想法对其进行打包!!!(没有转换), why? 就像上面转换单个文件的时候我们使用了插件, 所以，我们现在也要使用对应的规则插件对代码进行转换。

在上面代码中,我们使用了 const 和 箭头函数 ,所以我们需要下载这两个对应的插件;

```js npm
npm install @babel/plugin-transform-arrow-functions @babel/plugin-transform-block-scoping -d
```

然后在配置文件中加以使用

```js webpack
{
  test: /\.js$/,
  use: {
    loader: "babel-loader",
    options: {
      plugins: [
        "@babel/plugin-transform-arrow-functions",
        "@babel/plugin-transform-block-scoping",
      ]
    }
  }
}
```

但是我们又会发现, 我们这样使用的话，我们要配置多少啊 :sneezing_face:,所幸官方给我们配置好了
“规则集”/“预设”, 那么来上手试试呗。

```js 安装
npm install @babel/preset-env
```

```js 配置
{
  test: /\.js$/,
  use: {
    loader: "babel-loader",
    options: {
      presets: [
        "@babel/preset-env"
      ]
    }
  }
}
```

然后去运行使用。

:::info
和 postcss 一样，babel 本身就是一个工具集, 所以同样我们也可以将它的配置提到外部
:::
`.babelrc.json`/`babel.config.js` 两种文件名, 前者是较为早期的, 现在比较推荐后者

```js 外部配置
module.exports = {
  presets: ["@babel/preset-env"],
};
// webpack.config.js内部
{
  test: /\.js$/,
  loader: "babel-loader"
}
// 提取出来的只有配置 options
```

##### webpack 配置 Vue

对.vue 文件进行处理

```js 下载
npm install vue
npm install vue-loader -d
```

然后配置 vue-loader 对.vue 文件进行处理

```js 配置
{
  test: /\.vue$/,
  loader: "vue-loader"
}
```

并且我们还需要一个插件对 "< template >"进行处理

```js npm
npm install @vue/compiler-sfc -d
```

在配置文件中

```js webpack
const { VueLoaderPlugin } = require("vue-loader/dist/index");
new VueLoaderPlugin();
```

##### webpack devServer

使用 webpack 启动一个本地的服务器, 这个服务器的作用是什么呢?

- 实时编译并展示
- 实现请求跨域代理转发 (这个是我之前的一个盲点，之前一直不理解)

###### 如何搭建 devServer

三种方法

- webpack watch mode
- webpack-dev-server(最常用)
- webpack-dev-middleware(笔者现在还不会:sweat_smile:)

1. webpack watch mode

- 在配置中写入 `watch:true`,

  ```js 配置
  module.exports = {
    watch: true,
    ...
  }
  ```

- 在启动打包时使用`webpack --watch`

:::info
其实这个方法只能检测文件改变重新编译, 并不能实时刷新浏览器
:::

2. webpack-dev-server

首先要下载这个工具

```js 下载
npm install webpack-dev-server -d
```

然后在命令行中输入 `webpack-dev-server` 就可以试试效果了
这个不是关键, 关键是我们下面讲的它支持的 一些特性:

1. **静态资源引用**
   我们之前在使用 pubilc 中的资源的时候, 我们一次又一次的将它进行复制。。。这个是一个十分消耗内存和影响速度的隐患。 所幸, 我们可以在 devServer 中对其进行一个 **引用**, 而不必要每次去复制。server 服务器在请求资源的时候, 在打包的文件夹中找不到对应的资源, 就会去静态资源文件中对其进行使用
   ```js 配置
   module: {
   }
   devServer: {
     static: "./public";
   }
   ```
2. **热模块替换(HMR)**
   什么是热模块替换？ https://webpack.docschina.org/concepts/hot-module-replacement/
   :::info
   模块热替换(HMR - hot module replacement)功能会在应用程序运行过程中，替换、添加或删除 块，无需重新加载整个页面
   :::
   如何使用?https://webpack.docschina.org/guides/hot-module-replacement

   +++primary 如何使用 HMR

   1. 启用 HMR

   ```js 配置
   devServer: {
     static: "./public",
     hot: true
   }
   ```

   2. 但是我们发现启动后，它并没有使用模块热替换这个功能。why?原来我们还应该去指定那个模块使用 HMR。。。so, 如何做呢？
      在 index.js 中添加下面代码:

   ```js index.js
   if (module.hot) {
     module.hot.accept("./print.js", function () {
       console.log("Accepting the updated printMe module!");
     });
   }
   ```

   这样的话就会为`print.js`启动 HMR

   3. :upside_down_face:不会吧,我们不会每个都要去配置一下吧?
      对于我们单独配置的 js 或者其他模块就是这样, 但是对于.vue 来说, 处理 vue 的 vue-loader 就会默认做 HMR 的包装,不用我们管

   +++

3. **其他**

   - host : 设置主机地址
   - port : 监听的端口
   - open : 是否自动打开浏览器
   - compress : 是否为静态文件开启 gzip

4. **Proxy**
   :::info
   这个是我一直不太理解的一块, 我之前就知道它的概念, 就是对发出的请求进行代理,然后使用代理服务器发送请求, 跳过跨域问题。 我当时对这个代理的服务器就不理解, 我一直以为是网上的另外一个服务器。。。
   :::
   但这里的服务器其实是指,我们本地启动的 server 服务器, 跨域问题是浏览器单独有的一个东西, 但是 server 服务器又不是在浏览器.所以就巧妙的跳过了
   https://webpack.docschina.org/configuration/dev-server/#devserverproxy

```js webpack
proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,// 安全,
        changeOrigin: true
      }
    }
```

##### Resolve(解析)

设置模块如何被解析, webpack 提供合理的默认值, 但是还是有可能会修改一些解析的细节

1. alias 别名
   给我们的一些路径起个别名, 防止层级过深，出现`../../../../`的发生

```js
module:{},
resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }
  },
```

2. extensions 拓展后缀名
   尝试按顺序解析这些后缀名。如果有多个文件有相同的名字，但后缀名不同，webpack 会解析列在数组首位的后缀的文件 并跳过其余的后缀。

```js webpack
resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }
  },
```

3. modules 模块
   告诉 webpack 解析模块时应该搜索的目录。
   默认是只有 `node_modules`, 但是如果我们另外的人为引入一个包,放在了另外一个文件夹中; 我们不想去使用相对路径去找到它。那么我们就可以在这里进行一个配置

```js webpack
module.exports = {
  //...
  resolve: {
    modules: ["abc", "node_modules"],
  },
};
```

##### 开发与生成环境的区分

我们之前学了那么多，之前的一些功能慢慢的我们就不使用了, 其实是因为我们之前有些功能是在开发时使用, 一些是在打包环境的时候使用。所以我们总不能在开发的时候写一套, 然后打包的时候全部删除重写吧...其实我们一般是写两个文件, 在进行不同的操作的时候,去运行不同的文件。

新建一个 `config` 文件夹 , 在其中写两个文件 --webpack.dev.config.js--, --webpack.prod.config.js--, 其中 dev 为开发版本, prod 为打包版本。其中我们发现,我们又有很多的公用代码, 按我们写代码的思维, 肯定是要将其进行一个代码抽离,所以再创建一个 --webpack.comm.config.js-- 文件

```js webpack.comm.config.js
// webpack.comm.config.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const { VueLoaderPlugin } = require("vue-loader/dist/index");

module.exports = {
  target: "web",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../build"),
    filename: "js/bundle.js",
  },
  resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "../src"),
      js: path.resolve(__dirname, "../src/js"),
    },
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      {
        test: /\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]",
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024,
          },
        },
      },
      {
        test: /\.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]",
        },
      },
      {
        test: /\.js$/,
        loader: "babel-loader",
      },
      {
        test: /\.vue$/,
        loader: "vue-loader",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈",
    }),
    new DefinePlugin({
      BASE_URL: "'./'",
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false,
    }),
    new VueLoaderPlugin(),
  ],
};
```

```js webpack.dev.config.js
// webpack.dev.config.js
const { merge } = require("webpack-merge");

const commonConfig = require("./webpack.comm.config");

module.exports = merge(commonConfig, {
  mode: "development",
  devtool: "source-map",
  devServer: {
    contentBase: "./public",
    hot: true,
    // host: "0.0.0.0",
    port: 7777,
    open: true,
    // compress: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": "",
        },
        secure: false,
        changeOrigin: true,
      },
    },
  },
});
```

```js webpack.prop.config.js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");
const { merge } = require("webpack-merge");

const commonConfig = require("./webpack.comm.config");

module.exports = merge(commonConfig, {
  mode: "production",
  plugins: [
    new CleanWebpackPlugin(),
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "./public",
          globOptions: {
            ignore: ["**/index.html"],
          },
        },
      ],
    }),
  ],
});
```

然后我们在使用的时候, 在 package.json 文件中进行配置

```json package.json
"scripts": {
    "build": "webpack --config ./config/webpack.prod.config.js",
    "serve": "webpack serve --config ./config/webpack.dev.config.js"
  },
```

#### 引用

- <a href="https://juejin.cn/post/6923918805722726413#comment">webpack5 基础入门</a>
- <a href="">coderwhy</a>
