---
title: Hexo极速搭建个人博客
tags:
  - Hexo
  - 建站
  - 个人博客
  - Travis CI
  - GitHub Pages
  - Markdown
categories:
  - 教程
  - 工具
date: 2019-09-22 21:54:50
---


# 什么是Hexo？

Hexo 是一个快速、简洁且高效的博客框架。

Hexo 使用 Markdown解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

# 准备工作

**Markdown**

Markdown是一个极为简单的标记语言。

只要记住几个简单的标记，并在编辑文档的时候适当使用，文档就能被Markdown解释器识别并生成统一的格式。

能够识别Markdown格式文档的应用很多，比如有道云笔记，简书，还有本文提到Hexo。

下面的连接是一个Markdown在线编辑器，里面已经包含了Markdown的所有语法，10分钟你就可以基本掌握它。
- [Markdown在线编辑器 MdEditor](https://www.mdeditor.com/)

**Node.js**

Hexo是基于node.js编写的框架，安装和运行都需要先安装node.js环境。

简单使用Hexo并不需要掌握node.js，我们安装它仅仅是为了能运行Hexo。
- [Node.js 安装配置 | 菜鸟教程](https://www.runoob.com/nodejs/nodejs-install-setup.html)

**Git**

注册一个Github账号中，为个人博客创建一个repository。
把Hexo生成的站点文件提交到repository后，就可以通过下面的url来访问你的个人博客了。

```
https://<你的 GitHub 用户名>.github.io/<repository 的名字>
```
- [应该如何使用Github Pages?](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Using_Github_pages)

# 创建Hexo站点目录

安装node.js环境后在命令行执行npm命令安装Hexo。

```shell
$ npm install -g hexo-cli
```

Hexo 安装完成后就可以在命令行使用Hexo的命令来创建自己的站点了。

hexo init命令会在当前目录下创建名字为&lt;folder&gt;的文件夹，里面存放的就是你的Hexo站点文件了。

```shell
# <folder> 替换为你自己定义的名字
$ hexo init <folder>
$ cd <folder>
$ npm install
```

Hexo站点文件夹的目录结构如下。

```shell
.
├── _config.yml     # Hexo站点配置文件，大部分配置参数都在这个文件中配置
├── package.json    # node.js应用程序的配置信息，可以不用理会
├── scaffolds       # 模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件
├── source          # 存放用户编写的源文件，既我们自己写的Markdown格式文章
|   ├── _drafts     # 未发布的文章草稿的
|   └── _posts      # 已发布的文章
└── themes          # Hexo站点的主题
```

Hexo就是根据source/_posts下的Markdown文件来生成静态网页并存放到public目录下。

# 修改Hexo站点配置信息

打开_config.yml配置文件，修改Site信息，其他保持默认即可。
```
# Site
title: Tech-Tea         # 网站标题
subtitle: 茶话技术          # 网站副标题          
description: 个人技术博客   # 网站描述
keywords: 技术,Java,后端    # 网站的关键词。使用半角逗号 , 分隔多个关键词。
author: Drol Chan           # 博客作者名字
language: zh-CN             # 网站显示的语言
timezone: Asia/Shanghai     # 网站所在地区时区
```

# 本地启动Hexo站点

在站点目录下执行hexo server命令启动服务，默认监听4000端口，访问 http://localhost:4000/ 就可以看到我们的博客站点了。

```shell
$ hexo server -p 4000 # 不加 -p 命令默认监听4000端口
```

可以看到有一篇默认的文章，是创建站点目录的时候默认生成的文章。把source/_posts目录下的hello-world.md删除就能删除这篇文章了。

<img src="http://picbed.catfoodworks.com/tech-tea/hexo-quick-start/img001.png" width="800px">

# 开始发表文章

## 创建文章

创建的命令有两种方式

**第一种**

在站点目录下执行new命令可以创建文章，以&lt;title&gt;.md为文件名。

new命令创建的文章在source/_post目录下，相当于文章直接发布了。

```shell
$ hexo new <title> # <title>替换为文章的标题
```

**第二种**

在new命令后面加上draft，文章会保存在source/_draft目录下，相当于文章存为草稿。

```shell
$ hexo new draft <title>
```

启动Hexo站点的时候加上draft参数才会把草稿展示出来，默认不展示source/_draft目录下的文件

```shell
$ hexo server --draft
```

使用publish命令就可以把草稿文章移动到source/_post目录下，把文章发布。

```shell
$ hexo publish <filename> # <filename>替换为要发布文章的文件名
```

## 编辑文章

使用文本编辑器打开Markdown文件就可以开始写文章了。

在文件最上方 *---* 分隔的区域可以为当前文章指定Front-matter配置参数 

<img src="http://picbed.catfoodworks.com/WX20190922-182236@2x.png" width="500px">

**Front-matter 参数说明**

|参数 | 描述 | 默认值 |
| :- | :-  | :- |
|layout | 布局 | |
|title | 标题 | |
|date | 建立日期 | 文件建立日期 |
|updated | 更新日期 | 文件更新日期 |
|comments | 开启文章的评论功能 | true |
|tags | 标签 | |
|categories | 分类 | |
|permalink | 覆盖文章网址 | |
|keywords | 仅用于 meta 标签和 Open Graph 的关键词（不推荐使用）| |

# 生成站点

hexo server 命令的方式启动Hexo站点是框架动态解析sourc目录来展示我们的文章的，正式发布站点前我们需要把我们的站点生成纯静态的网页。

需要执行generate命令生成站点，执行命令后会在根目录创建一个public目录，里面是包含了我们所有文章的静态站点。

```shell
$ hexo generate
```

# 部署站点

最简单的方式，把public提交到我们github上为博客创建的repository。

就可以通过url https://<你的 GitHub 用户名>.github.io/<repository 的名字> 在互联网上访问我们的站点了。

但是这样的部署方式很不优雅，只有静态网页保存在repository里，最重要的source目录并没有提交。

## Travis CI

Travis CI 是一个公共的持续集成服务。

只需把我们的github账户授权给它，然后在我们源码根目录添加.travis.yml配置文件。

就可以实现对master分支push代码的时候自动为我们生成并部署静态站点。

Travis CI 会拉取我们的代码，然后按照.travis.yml文件的配置，在他们的服务器上执行Hexo命令生成静态站点文件。

Travis CI 生成静态站点后，会把静态站点文件提交到gh-pages分支下（需要自己提前创建这个分支）。

在我们的GitHub Pages配置里指定gh-pages分支作为发布分支。

现在可以使用url https://<你的 GitHub 用户名>.github.io/<repository 的名字> 在互联网上访问我们的站点了。

具体的操作步骤可以参考Hexo官网的相关文档 
- [将 Hexo 部署到 GitHub Pages](https://hexo.io/zh-cn/docs/github-pages)

## 一个小小的bug fix

站点部署好后，你可能会发现浏览器左下角一直显示加载中（如果你使用chrome）的话。但是看起来页面已经加载完毕了。

用开发者工具查看网络请求的话就会发现，一直在请求 http://ajax.googleapis.com/jquery/2.0.3/jquery.min.js ，这是因为默认主题使用的jquery的cdn是谷歌的，被墙了= =

需要到 theme/landspace/layout/_partial/ 目录下修改after-footer.ejs文件。

把第17行 *//ajax.googleapis.com/jquery/2.0.3/jquery.min.js* 替换成 *//cdn.bootcss.com/jquery/2.0.3/jquery.min.js* ，使用国内的jquey cdn

## 总结

本文只是介绍了最快的方式利用Hexo来搭建自己的个人博客，Hexo本身还有很多可以自定义的地方，比如更换站点的主题，自己定义页面和模板。

更多的Hexo的相关内容请移步Hexo官网学习
- [Hexo 官网](https://hexo.io/zh-cn/)
