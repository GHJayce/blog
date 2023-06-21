---
title: "记录hugo快速上手搭建一个静态博客"
slug: hugo-quick-start-and-build-blog
date: 2023-06-20T23:46:50+08:00
image: https://repository-images.githubusercontent.com/11180687/9d3d8200-abf2-11e9-803c-4cdfde0d22e5
categories:
    - 工具
tags:
    - hugo
    - 静态站点生成器
    - 教程
---

## 前言
> 开头废话有点多，[点我进入正题](#快速入门)
### 为什么想要搭建一个博客？
说起来有很多方面的因素，最重要的目的是希望能够留下一些技术积累和记录可以给别人看，同时提升一下写作的能力，毕竟与平时记笔记的方式完全不同。
> 虽然也没有什么人看:person_shrugging:

其实在此之前也有写过博客，比如早期使用`heroku` + `php`搭建的个人博客，因为需要不断开发完善功能，加上后来也不免费了，索性不折腾直接上博客网站上写博客，如：[CSDN](https://blog.csdn.net/JayceDeng)、[Segmentfault](https://segmentfault.com/u/jayce_)等。

但这些平台使用下来以后也发现了一些问题，总的来说就是不适合自己的使用，写文章的过程也不够简洁直接，不够纯粹。

### 我理想中的博客是什么？
1. 专注于markdown的编写
1. 支持全文检索
1. 具有标签功能
1. 能够归档文章
1. 免费
1. 支持永久存储

### 为什么选择了hugo？
查阅资料得知，静态站点生成器有三种提的比较多，分别是：
1. [jekyll](https://jekyllrb.com/)，基于`Ruby`语言编写
1. [hexo](https://hexo.io/)，基于`Node.js`编写
1. [hugo](https://gohugo.io/)，基于`Go`语言编写

之所以选择`hugo`，它是当中编译速度最快、文档相对友好、star数量还多，正好无意间找到[Hugo Themes](https://themes.gohugo.io/themes/hugo-theme-stack/)的主题，很好的契合了需求，就决定是它了。

### 阅读前提
本文默认你已经掌握了以下的几个点：
1. 了解[github pages](https://docs.github.com/zh/pages/getting-started-with-github-pages/creating-a-github-pages-site)并且会配置
1. 熟悉使用`git`

通过该文章可以快速上手使用hugo，所以有一些内容不会详细展开。

## 快速入门
### 安装Hugo
这里我用的是Mac系统，所以采用`Homebrew`的方式进行安装，其他安装方式自行[查阅](https://gohugo.io/installation/)。
```bash
brew install hugo
hugo version
```
### 创建项目
```bash
hugo new site your-project-name
cd your-project-name
git init
```
> 由于我已经有`git`项目`ghbjayce.github.io`，里面没有任何东西，所以这里我使用了`hugo new site GHBJayce --force`，既不影响`hugo`的生成又不影响`git`的项目。

执行完以后，你会得到以下的目录结构：
```
├── archetypes
│   └── default.md
├── assets
├── content
├── data
├── hugo.toml
├── layouts
├── static
└── themes
```

### 安装主题
由于新建的项目没有默认主题，我们从[主题库](https://themes.gohugo.io/)把挑好的主题下载下来。
```bash
git clone git@github.com:CaiJimmy/hugo-theme-stack.git themes/hugo-theme-stack
cp -r ./themes/hugo-theme-stack/exampleSite/content/* ./content
cp ./themes/hugo-theme-stack/exampleSite/config.yaml ./hugo.yaml
rm -f ./hugo.toml
# 以下是示例主题需要操作的命令，避免没有翻墙的情况下启动不了项目
rm -rf ./content/post/rich-content
```

> [站点配置](https://gohugo.io/getting-started/configuration/)文件为`hugo.toml`，支持三种后缀的格式：`toml`、`yaml`、`json`，这里我选择用`yaml`

### Hello World
生成一篇文章。
```bash
# 对应的生成位置：/your-project-name/content/post/first-post.md
hugo new post/first-post.md
```
打开它写点内容。
```markdown
---
title: "First Post"
date: 2023-06-20T17:45:58+08:00
draft: true
---

# hello world
```

### 启动
```bash
# 执行成功以后，终端会出现server的地址，例如：http://localhost:1313/
hugo server --buildDrafts
```
> `--buildDrafts`的作用是草稿文章（对应文章中的`draft: true`属性）也进行生成，去除的话会跳过生成草稿。

### 构建&发布
```bash
hugo
```
你会看到多出一个`public`目录，就是构建好以后的内容，里面的内容完全是静态的，放到站点下就能够访问。

但是我们要发布到`github page`上，根目录只支持`/docs`，所以我们得把`public`改成`docs`，[有两种方式](https://gohugo.io/getting-started/usage/#build-your-site)：
1. 构建时加上参数：`hugo --destination docs`
1. 在配置文件中加入参数：`publicDirectory: docs`
> 如果仓库只放`public`目录下的内容，那么可以忽略这里。

## 版本说明
文中相关软件版本说明：
1. hugo：v0.113.0
1. hugo-theme-stack：v3.16.0

## 参考
1. [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)