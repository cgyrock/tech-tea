---
title: Hexo第三方主题NexT
tags:
---

<img src="http://picbed.catfoodworks.com/WechatIMG322.jpeg" width="400px">

前几天第一次尝试使用Hexo在Github Pages上搭建自己的个人博客。
完成所有基本工作后，很快就意识到一个问题，官方默认主题真是贼JB丑= =
还好Hexo本身就是一个支持自定义主题和插件的框架，官网上也有很多第三方主题。

# 应用主题
应用第三方主题的方法也很简单。
只要把第三方主题下载下来，放到站点根目录的themes目录下。
修改站点根目录_config.yml里theme值为第三方主题名称就可以了。
主题根目录下也有一个_config.yml文件，
里面的配置项是针对主题配置的一些属性值和控制项，
大部分主题可以通过这个文件做一些个性化定制。

<!-- more -->

# NexT

<img src="http://picbed.catfoodworks.com/WeChat03efe61f3e08feacf8c4fced5b9ae3a7.png" width="800px">

官网上有200多个主题看得眼花缭乱。
最后直接网上搜，发现一款对入门很友好，对扩展也很友好的主题 - NexT
直接搜索Next大概率会搜到 http://theme-next.iissnan.com/ 这个中文网站。
一开始我以为这就是官网，后来发现这里Github项目是iissnan/hexo-theme-next。
应该是一个基于next作了一番定制的版本，而且好像不是基于最新版的Next。

真心觉得这个中文网的文档做得很漂亮，很容易阅读。但是因为个人洁癖的原因，还是希望能使用原版的next。
一番搜索，github上找到了原项目（其实是在上面那一版的配置文件里的注释发现了原版项目url地址）

- theme-next/hexo-theme-next

按照官网文档操作，在自己站点项目themes目录下执行git clone命令，把整个项目拉下来（这个操作挺坑的= =，后面说）。然后修改站点_config.yml文件

```
language: zh-CN # 设置主题语言为中文，注意如果是iissnan/hexo-theme-next这个项目，这个值是zh-Hans，不明白他们为啥要改这个值的定义= =。
theme: next # 指定站点使用主题为next
```

然后hexo serve启动服务器就可以在本地看到新的主题了！

# Next Scheme

<img src="https://d33wubrfki0l68.cloudfront.net/87468aa2a2f0adbbff8a6e0e6015d4649ca31c2d/af2b3/images/docs/next-schemes.png" width="800px">

Next这个主题本身支持四种scheme，就是主题预定义布局方式，个人比较喜欢Mist这个布局。
修改scheme很简单，打开next目录下的_config.yml文件，搜索Schemes，就可以找到对应的scheme配置项了，默认scheme是Muse

```shell
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

好了，发布到线上吧，git push，等待 travis CI 生成并发布。
通过域名访问自己博客，一看傻眼了，一片空白= =。

# git submodule

回到github上，看自己博客项目代码目录，themes/next 这个目录是空的！
想想也对，刚才next是通过git clone拉下来的别人的repository，当然不会提交到我的repository里啦。

一番搜索后，解决方法有两种，第一把next的.git删除了，那他就和原来repository没有关系了，只是文件目录。
但是这样感觉很不优雅，在项目里提交一堆不是自己写的代码。而且日后想要升级，只能通过拷贝的方式。

第二种方法就是使用git submodule命令，把next作为博客项目的子模块引入，并不会把next的代码包含到博客项目里，而是创建一个引用到next项目。
当然，我肯定还要修改next项目里的_config.yml，所以不能直接引用next项目，因为我没有修改权限。
要先去next的github页面上fork一份到自己账号下，这样我就能修改了，然后以后有更新就直接从next项目拉取到我这个fork的项目上就行了。

回到自己站点项目的themes目录下，把next目录删除，执行命令，添加自己fork的next库为子模块
```shell
$ git submodule add https://github.com/cgyrock/hexo-theme-next.git next
```

然后切换到next目录下，创建一个分支，然后在这个分支上修改配置。这时git commit提交的代码是提交到我fork的next项目里的。

回到站点目录的根目录，要执行子模块更新命令，站点项目才会最终到子模块repository的变化。
```shell
$ git submodule update 
```

<img src="http://picbed.catfoodworks.com/WechatIMG323.jpeg" width="800px">

这个时候总算是完成了，git push，触发travis CI，访问博客，终于看到主题更新了。