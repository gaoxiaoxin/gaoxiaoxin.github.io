---
title: MarkDown的使用
data: 2020-06-01
categories:
  - [杂项]
---

# MarkDown 使用指南

<!--more-->

[TOC]

## 1. MarkDown 简介

![image-20201119201957156](https://tva1.sinaimg.cn/large/0081Kckwly1gkuqzjkidej303m03aa9x.jpg)

- Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。

- Markdown 语言在 2004 由约翰·格鲁伯（英语：John Gruber）创建。

- Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。

- Markdown 编写的文档后缀为 **.md**, **.markdown**。

## 2. MarkDown 编辑器

有很多编辑器可以用来书写 MarkDown，比如我们常用的 VSCode 代码编辑器，安装了支持 MarkDown 的插件就可以用来书写 MarkDown 文档。

这里我介绍一个非常好用的编辑器，叫做: **Typora**

**下载地址**： https://typora.io/

![image-20201119202433251](https://tva1.sinaimg.cn/large/0081Kckwly1gkur4bs8jjj30la0903zh.jpg)

支持 MacOS,Windows 和 Linux 三个平台。

## 3. MarkDown 基本语法

### 1.标题

```
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

### 2. 文字强调

**粗体**

```
***粗体字*
```

_斜体字_

```
*斜体字*
```

**_粗体+斜体_**

```
***粗体+斜体***
```

### 3. 列表

**有序列表**

有序列表的格式是: 数字小数点+空格+文本

```
1. abc
2. bbc
3. ccc
```

**无序列表**

格式: 横线(星号) + 空格 + 文本

```
* abc
* bbc
* ccc
```

- abc
  - bbc
    - ccc

不管是有序还是无序都支持嵌套和多级列表：

- abc
  - bbc
  - ccc
- hello
  1. 你好
  2. 哈哈哈
  3. 😁

### 4. 链接

格式：方括号小括号(链接)

[百度](http://www.baidu.com)

```
[百度](http://www.baidu.com)
```

### 5. 图片

格式：感叹号+外部链接的格式。 链接支持相对引用！

![百度](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png)

```
![百度](https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png)
```

### 6. 水平分割线

格式: 空行 ---

---

```
hello

---

world
```

### 7. 引用

> 引用其他文字段落的使用可以使用

格式： 大于号(>) + 空格 + 内容

```
> Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档
```

### 8. 代码

**行内代码**

格式：反引号+内容+反引号

`console.log("hello world")`

**多行代码**

格式：三个反引号+内容+三个反引号

````
​```
function add(x,y){
	return x + y
}
​```
````

### 9. 表格

默认的列左对齐,也支持设置居中和右对齐, 横杆大于等于 3， 列的竖必须闭合才能代表一列！

- 默认对齐(左对齐) : `:----`
- 居中对齐 : `:---:`
- 右对齐: `---:`

| 姓名 | 年龄 |   地址 |
| :--- | :--: | -----: |
| 大卫 |  20  | 北京市 |

```
| 姓名 | 年龄 |   地址 |
| :--- | :---: | ---: |
| 大卫 |   20 | 北京市 |
```

### 10. 任务列表

展示待办事项效果, 可以认为是列表的变种，可以混合使用

- `- [ ] text` : 方括号内部空格代指是待办事项
- `- [x] text` : 方括号内部 x 代指事项已经完成

- [ ] 学习 markdown 语法
- [x] 跑步 🏃

```
- [ ] 学习markdown语法
- [x] 跑步🏃
```

### 11. 脚注

格式：

- 声明: `[^text]: description`
- 引用: `hello[^text]`

### 12. 删除线

格式：两个波浪线+文本+波浪线

~~删除的内容~~

```
~~删除的内容~~
```
