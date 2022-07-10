---
title: express项目入门（1）
sticky: true
categories:
  - ["Node"]
tag: ["后端", "Express"]
---

#### 前言

作为一个主修前端的程序修狗, 当我们想接触后端, 我们最好的选择就是去学 Node.js, 因为我们对于 js 足够的熟悉。所以学习 Node.js 的成本很低, 那么我们其实想学 Node.js, 最终目的在于去写一些后端的项目。或者使用 Node.js 发挥它脚本语言的优势去写一些小的脚本。
那么问题来了, 我们想写后端应用的时候, 我们一般都不会去使用它的原生去写,因为太过于复杂。
所以我们都会去选择一个框架去写项目,这里就回到主题,我们今天的主角 `express`;
:::primary
要是有所得罪请原谅, 本是出自一番好意,
只是想显点粗浅技艺, 那才是我们的初衷。
-- 威廉·莎士比亚，《仲夏夜之梦》
:::

#### 开始

##### express

首先给大家上官方的网站
<a href="https://expressjs.com/zh-cn/">Express</a>
官方网站中有提到, express 是一个 `高度包容`, 快速而`简约`的 Web 框架。那么我们如何去理解这句话呢?
:::info
高度包容 ==> 你可以去安装各种各样的包去扩展它的功能
简约 ==> 它只有简单的中间件和路由功能.
:::
小知识 ==> `koa` 甚至连路由功能都没带!!!
这里最重要的就是上面提到的那些东西. 其实很多人都不认为`express`是一个框架,因为它太轻了.

###### express 的中间件

`express`中间件, 就像一个过滤器一样,它会依次一层一层的过滤发送到`express`应用的请求,直到有中间件将请求进行了响应返回。中间件会影响发送进来的请求.
分类:

- 错误处理中间件
- 应用程序中间件(全局)
  例: 请求体和解析体的处理,

- 路由中间件
  处理你的路径, 为对应的路径加上中间件

:::primary
本文不会给你一点点的去讲解 express 的基本操作, 因为那实在是太基本了.本文倾向于基于项目去讲解 express 的集成
:::

##### express 项目开始

###### 启动服务&&添加基本配置

首先我们需要先新建一个项目 `express-template`, 然后我们用 `npm init -y`来初始化一个 npm 库, 它会添加一个文件 `package.json`, 这个文件会保留我们的项目的 npm 的依赖.
那么开始吧!!! 这里我们不打算使用 express 自带的项目生成器.我们自己手动创建!
首先下载 express

```xshell install
npm install express
```

接着初始化项目,创建项目的目录分工
![](https://tva2.sinaimg.cn/large/006xVoHRly8h2cvugx3anj305707zq32.jpg)
其中

- config ==> 是去写一些我们项目中要用到的基本的数据
- middle ==> 写项目中用到的一些应用中间件
- router ==> 写我们用到的路由
- service ==> 写我们对数据库的操作
- util ==> 写我们用到的一些工具函数
- validator ==> 写我们对接口数据的校验函数
- app.js ==> 我们项目的入口

然后让我们先启动一个简单的`express`项目;

```js start
const express = require("express");

const app = express();

const PORT = "8000";

app.get("/", (req, res) => {
  res.end("Hello World");
});

app.listen(PORT, () => {
  console.log("server is running in http://localhost:" + PORT);
});
```

在项目目录下输入`node app.js`来启动项目,然后我们访问`http://localhost:8000`这样的话,正常我们就可以看到一个`Hello World`输出。
这样的话, 我们就启动了一个最基础的项目。
我们可以看到在上面的基础的项目中, 我们用到了`node`去启动项目, 用到了 `PORT`这个属性.
那么我们依次来看, 我们可以发现, `node`启动的项目只能启动一次,当项目有了改变之后, 并不会实时进行修改并重新启动项目, 那么我们就可以用`nodemon`这个库来解决上述问题。

```xshell nodemon
npm install nodemon -d
```

然后我们就可以在项目目录文件夹下运行 `nodemon app.js`, 来启动项目。(这样子,没有那种写项目的感觉), 所以我们添加一个 npm 脚本命令, 在`package.json`中添加一个基本的命令

```json package.json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "nodemon app.js"
},
```

然后我们就可以使用 `npm run start`这个命令来启动我们的项目了,是不是有点感觉了

那么接下来,我们去看 `PORT` 这个属性。其实这个属性其实是应该放在 `config`文件夹中的, 因为它是一个基础数据, 我们放在`config`中对它进行统一修改.但是我们需要考虑另外一个事情, 那就是它的可拓展性, 和它的保密性。那么我们这里推荐`dotenv`库,这个库会将项目目录中的`.env`中写的数据添加到环境变量中. 在上传项目到 GitHub 上的时候, 我们在项目的`.gitignore`文件中声明不上传`.env`文件, 来实现对项目机密数据的保密性。

```xshell dotenv
npm install dotenv -d  // 安装dotenv库
git init // 初始化git项目
```

新建 `.gitignore`文件, `.env`文件
![](https://tva4.sinaimg.cn/large/006xVoHRly8h2cx87umy4j30a7043wef.jpg)
![](https://tva2.sinaimg.cn/large/006xVoHRly8h2cxbduw08j30am05vmx2.jpg)
然后这个时候就用到了我们的`config`文件夹, 在其中新建 `index.js`, 我们在其中完成其他部分

```js config-index.js
require("dotenv").config(); // 将.env文件中的数据注入到环境中
const PORT = process.env.PORT || 8080;
module.exports = {
  // 将环境中的变量取出, 做一个中转
  PORT,
};
```

然后在使用到这些数据的地方进行导入使用就好。

###### 路由的配置&&另外的一些配置

我们之前在我们最基本的项目中写到了一个基本的路由, 是 method 为 get 的一个请求。那么我们写后端项目的话, 我们不可能只写一个接口, 当我们接口增多的时候, 我们就会发现我们的项目代码很乱.那么这样的话我们的 `router`接口就派上用场了。我们来看看如何使用,

1. 我们先要去了解一下: 路由处理程序,
   路由中间件中我们可以提供多个回调函数, 以类似中间件的行为方式处理请求.路由处理程序的形式可以是一个函数, 一组函数或者两者的结合.
   例： (其实是官网的例子)

```js demo
app.get("/example/a", function (req, res) {
  res.send("Hello from A!");
});
app.get(
  "/example/b",
  function (req, res, next) {
    console.log("the response will be sent by the next function ...");
    next();
  },
  function (req, res) {
    res.send("Hello from B!");
  }
);
// 其他的例子请看官网 https://expressjs.com/zh-cn/guide/routing.html
```

我们项目的核心也就是基于路由的处理机制来编写.

2. 微型应用程序, 可安装的模块化路由处理程序.
   https://expressjs.com/zh-cn/guide/routing.html(去官网看看, 这里不多做解释.)
   我们使用 `express.Router` 类创建的可安装的模块化路由处理程序.
   在我们的`router`文件夹中 新建`index.js`, `user.js`
   `index.js`是一个中转类型的文件, 把我们的小的路由应用程序挂载到一个大的路由应用程序中. 实现在 app 上的统一挂载. `user.js`这个就是我们写的一个简单的用户模块, 我们来看看

```js user.js
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.send({
    message: "获取到一条用户信息",
  });
});
module.exports = router;
```

```js index.js
const express = require("express");
const router = express.Router();

router.use("/user", require("./user"));

module.exports = router;
```

```js app.js
const express = require("express");
// 导入基本数据
const { PORT } = require("./config");
// 导入路由模块
const router = require("./router");
// 创建app实例
const app = express();
// 将路由实例进行挂载
app.use(router);
app.listen(PORT, () => {
  console.log(`server is running in http://localhost:${PORT}`);
});
```

然后当我们去请求`http://localhost:8000/user/`的时候,我们就能后就能收到我们的响应了.
之后我们去写接口的话, 就分模块去添加我们的接口就行.
这里我们写了一个简单的 get 请求, 我们在日常的开发中, 还会有很多的不同类型的接口.
我们这里写一个 post 请求（login）看一下,使用 json 传输数据(username, password).
但是我们去写接口的时候,我们就会发现我们其实不知道, 传递过来的 json 数据会挂载到那?
那么怎么办呢, 这里我们就需要两个 express 自带的中间件

- express.json() 这个是对传入的 json 数据进行解析
- express.urlencoded() 这个是对传入的 x-www-form-urlencoded 类型的数据进行解析

:::primary
不用再额外的安装 body-parser 插件, express 为我们集成了这个东西.
:::

那么对数据上传比较熟悉的同学, 就会想到我们还有另外的一个类型 `form-data` 类型的数据.这种类型的数据就是用外部的中间件包来实现了 `multer`.这个我们后面再聊,这个最多的是用于处理文件上传。
那么我们在`app.js`中使用上面提到的两个中间件, 这两个中间件会将我们传入的参数添加到`req.body` !!! 这个是在中间件中创建的, 不是自带的.

```js app.js
app.use(express.json());
app.use(
  express.urlencoded({
    extended: true,
  })
);
// 请求数据处理中间件,放在路由中间件前.
app.use(router);
```

这里我们在 `urlencoded`这个中间件中传入了参数 `{extended:true}`
这里的话如果参数是 true, express 就会使用内置的第三方库 fs 来进行解析.
如果参数是 false, express 就会使用 Node 内置模块 querything 来进行解析.
如果你不加这个参数, 会导致
![](https://tva3.sinaimg.cn/large/006xVoHRly8h2dj2mfvz2j30i401lwep.jpg)
那么现在就该提速了.

###### 连接数据库

我们写后端项目, 就一定会去接触数据库这个东西, 那么我们这里使用 mysql 数据库, 这样我们选择的话, 我们就要选择 mysql 的数据库驱动 ==> <a href="https://www.npmjs.com/package/mysql2">mysql2</a>（是数据库驱动 mysql 的升级） 我们这里不使用 mysql2 中提供的简单连接, 我们直接使用它提供的连接池. 具体的可以去官网学一下.

```xshell mysql2
npm install mysql2
```

+++primary 什么是连接池,它有什么作用?
数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。 这项技术能明显提高对数据库操作的性能。
+++
我们先创建我们的数据库连接池.
在`service`文件夹中我们新建 `index.js`文件, 在这个文件中写入

```js 数据库连接池
const mysql = require("mysql2");
const { PORT } = require("../config");
const pool = mysql.createPool({
  host: host, // 数据库的地址
  user: user, // 用户名
  database: database, // 要连接的数据库
  password: password, // 密码
  waitForConnections: true, // 等待连接
  connectionLimit: 50, // 最大连接数
  queueLimit: 0,
});

pool.on("acquire", function (connection) {
  console.log(`获取数据库连接 [${connection.threadId}]`);
});
pool.on("connection", function (connection) {
  console.log(`创建数据库连接 [${connection.threadId}]`);
});
pool.on("enqueue", function () {
  console.log("正在等待可用数据库连接");
});
pool.on("release", function (connection) {
  console.log(`数据库连接 [${connection.threadId}] 已释放`);
});
module.pool = pool;
```

这里的话看到我们其实是用了很多的这些基础性数据, 那么我们是不是应该把它同样的放在环境变量中做一个保密性隐藏?

```js config index.js
require("dotenv").config();
const PORT = process.env.PORT || "8080";
const host = process.env.host || "localhost";
const user = process.env.user || "root";
const database = process.env.database || "demo";
const password = process.env.password || "123456";
module.exports = {
  PORT,
  host,
  user,
  database,
  password,
};
```

然后再在我们上面的项目中引入这些数据

```js demo
const { host, user, database, password } = require("../config");
```

然后的话,我们再对我们的连接操作进行一个封装

```js 数据连接封装
// 传入sql语句, 和args参数数组
module.exports = function getData(sql, args = []) {
  return new Promise((reslove, reject) => {
    pool.getConnection(async (err, conn) => {
      if (err) {
        reject(err);
      } else {
        try {
          const data = await conn.promise().execute(sql, [...args]);
          reslove(data);
        } catch (error) {
          reject(error);
        } finally {
          pool.releaseConnection(conn);
        }
      }
    });
  });
};
```

后面我们就用这个封装的函数去数据库进行操作.
来实际操作一下
我们在`user.js`中我们写一个中间件函数,（user 这个表是我已经写好的一个表）

```js user.js
const getData = require("./index");

exports.getAllUser = async (req, res, next) => {
  const [user] = await getData("select id from user");
  console.log(user);
  res.send({
    data: user,
  });
};
```

然后大家可以我这写了一个函数, 那么我们如何使用它呢? 回到我们刚才说的路由,我们想到之前我们想路由处理机制, 我们将这个函数直接放在我们的`user`的路由中.

```js router user.js
const express = require("express");
const router = express.Router();
const { getAllUser } = require("../service/user");
// 获取全部的路由
router.post("/getAll", getAllUser);
module.exports = router;
```

![](https://tva4.sinaimg.cn/large/006xVoHRly8h2dqpejqa8j30x00ax751.jpg)
这样的话,当我们去请求这个接口,我们就会发现我们的请求已经成功了。
:::primary
有点难受, 感觉还不如讲视频比较舒服, 写字好多东西都讲不出来.难受 ing
:::

本来是打算一篇文章写完的,但是。。。太多了啊.等我后面慢慢的写吧.
未完待续...

#### 结尾&&想说的话

之前就有学过 express, 但是是使用 mongoDB, mongoose 这些去写项目, 对于 ORM(对象关系映射模型)不是很理解, 整个人都是一个混乱的状态, 然后一个偶然的机会去学了 java + springBoot + mysql 写项目, 感谢孙老师, 了解到高级语言连接 mysql 的 MySQL 驱动,连接池,连接的建立和销毁等这些概念.加上这学期也在学习数据库, 就对这些东西有了一些另外的一些理解

:::primary
我也就只活一次, 开心最重要!
:::
