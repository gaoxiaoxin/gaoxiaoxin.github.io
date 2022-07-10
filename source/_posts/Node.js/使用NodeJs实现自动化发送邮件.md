---
title: 使用Node.js实现自动化发送邮件
sticky: true
---

### 使用Node.js实现自动化发送邮件

之前学到的一个东西，今天突然想起来，就来写了。回想自己学习前端已有快一年的时间了，但是真真的自己总结却没有太多。那就从现在开始，用现在作为一个契机。

#### 工具准备 
1. nodemailer插件
```javaScript nodemailer使用
安装 nodemailer 包
1. npm install nodemailer   
应用 nodemailer 
2. let nodemailer = require('nodemailer')
// 引入证书文件
let credentials = require('../config/credentials')
// 创建传输方式 
let transporter = nodemailer.createTransport({
  // 选用的邮箱服务， 笔者这里选用的是qq邮箱
  service: 'qq',
  // 给予插件qq相关数据
  auth: {
    user: YourUser, // 你的qq号
    pass: YourPass // 一个特定的编码
  }
});

// 注册发送邮件给用户 
exports.emaliSignUp = (email, res) => {  // res , 处理的参数
  // 发送信息
  let options = {
    from: '1831430227@qq.com',
    to: email,
    subject: '感谢您在该应用上注册。',
    html: '<span>欢迎您的加入</span><a href="http://localhost:8080/">点击</a>'
  };

  // 发送邮件
  transporter.sendMail(options, (err, msg)  => {
    if(err){
      res.send(res)
      console.log(err);
    }else {
      console.log('The message was already sent successfully');
      res.send('邮件发送成功！')
    }
  })
}
``` 
上述代码中的pass 是需要去qq邮箱打开POP3/SMTP服务
这里因为笔者又懒又烂。。。，就不上图了 ， 流程线来一套：

设置 -> 账户 -> POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务 -> 开启服务... 你会看见的

开启后，点击下方的**生成授权码** 就可以

### 参考
1. https://www.w3schools.com/nodejs/nodejs_email.asp

### 结语
文章虽短，但是却是一个好的开始