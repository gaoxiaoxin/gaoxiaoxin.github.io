---
title: TypeScript基础总结
---

#### 前言
之前感觉学Ts的时候，自己做了笔记就万事大吉，结果。。。呵呵(我真是个可爱的傻子)

#### 什么是TypeScript？
TypeScript is a strongly typed programming language that builds on JavaScript, giving you better tooling at any scale.
TypeScript是JavaScript的一个超集，主要提供了类型系统和对ES6的支持。防止了Javascript之前对类型的不检测，可以在编译期间发现并纠正错误，支持模块，泛型和接口

#### TypeScript初体验
1. 安装Typescript
> npm install -g typescript
2. 验证是否安装上
> tsc -v 
> Version 4.4.3
3. 创建文件 helloWorld.ts，输入代码
> let message: string = 'Hello Typescript'
> console.log(message)
4. 编译文件
> tsc helloWorld.ts
> .ts -> .js

那我们这样的是将ts编译为js再去运行，那我们如何让它更便捷的让我们使用呢？
回想一下，js可以在node环境下运行，那么ts呢？答案是肯定的，那就是  ts-node 
1. 安装 ts-node 
> npm install -g ts-node 
2. 下载依赖插件
> npm install -D tslib @types/node
3. 测试使用
> ts-node helloWorld.ts

#### TypeScript数据类型
##### 原始数据类型
###### 1. Boolean类型
```TypeScript Boolean 
let isDone: boolean = false 
console.log(isDone)
```
###### 2. number类型
number 类型既可以表示 十进制，也可以表示二进制，八进制，十六进制
```TypeScript Number
// 十进制
let num: number = 100
// 二进制
let num2: number = 0b100
// 八进制
let num3: number = 0o100
// 十六进制
let num4: number = 0x100
console.log(num, num2, num3, num4)
```
###### 3. string类型
```TypeScript string
let str: string = "Hello TypeScript"
let ts: string = "World"
let hello: string = `Hello ${ts}`
```
###### 4. null和undefined
```ts null和undefined
let u:undefined = undefined
let n:null = null
```
他们不能相互赋值，会引起报错。他们的值只有自己的类型