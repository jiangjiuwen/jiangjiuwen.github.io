---
title: 如何在GitHub上构建并部署Hexo博客
categories: 编程之美
date: 2017-04-22 20:45:12
tags:
- Hexo
- GitHub
keywords: Hexo,GitHub
---
当你在搜索引擎中同时输入`Hexo`、`GitHub`等关键字，浏览器会立马展现无数教你利用GitHub和Hexo构建免费个人博客的教程，所以这里不再对此赘述，本文旨在记录我这两天搭建博客的过程中遇到的问题以及解决办法。

# 环境配置

首先当然是安装Node.js，Hexo以及Git(因为要部署博客和托管源码到GitHub)，几条命令就解决了，轻松加愉快。

以下是Hexo官网提供的几条最基本的使用命令。

``` shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

值得注意的是，如果是从远程仓库`pull`下来的源码，则不需要初始化Hexo文件夹，只需要执行第四行`install`命令就够了，因为远程仓库中的文件是已经初始化过的，如果再次`init`会使自定义配置文件被默认文件覆盖。
<!-- more -->
# 绑定独立域名

当我们在创建仓库的时候GitHub就自动给我们生成了一个二级域名，比如`jiangjiuwen.github.io`，但是，作为个人博客，拥有一个自定义的独立域名将是一件多么炫酷的事情(购买顶级域名，这是唯一一件需要花钱的事情)。

## 修改DNS

到域名管理面板增加两条A记录和一条CNAME，如下所示。

```
A @ 192.30.252.153
A @ 192.30.252.154 
CNAME www jiangjiuwen.github.io
```

## 创建CNAME文件(无后缀)

该文件里面有且仅有一行文本(你的独立域名，比如`jiangjiuwen.com`，没有`http`前缀)，需要一同被部署到GitHub。那么问题来了，当我第二次部署的时候，这个文件不见了，此时独立域名失效，其实这是因为每次部署都会更新整个仓库，而CNAME文件是用户手动添加的，所以被删除之后不会自动生成。

为了解决这个问题，只需要将CNAME文件放入`blog/source`文件夹下，然后再部署，此时CNAME文件就会随博客一起部署了，同理，其他由用户手动创建并需要被部署的文件都需要放在该文件夹下。

# 多设备协同工作

通常我们只会在一台电脑上工作，但是，有时候我们也会需要在另一台电脑上写工作的情况，如果重新配置环境，就无法在当前博客版本上生成博客文件，这是无法接受的。

因此，我在把博客部署到Repo的`master`分支的时候，同时会把源代码一并`push`到另外一个branch上(比如本博客的`src`分支)，这样当我们在另外一台电脑上配置环境的时候只需要`pull` `src`分支就可以在最新的版本上继续工作了，部署之后将修改的内容一并`push`到分支上，这样回到主工作电脑上就可以将最新的修改`pull`到本地继续工作。

# 备忘录

搭建博客的过程中学会了很多命令，包括一些常用git命令和hexo命令，避免忘记，在此记录一下。

## 常用Git命令

连接远程服务器

``` shell
$ git remote add origin git@github.com:jiangjiuwen/jiangjiuwen.github.io.git
```

从服务器获取src分支代码

``` shell
$ git pull origin src
```

检查当前分支状态

``` shell
$ git status
```

将当前所有修改管理起来

``` shell
$ git add .
```

提交本地修改到本地仓库的当前分支

``` shell
$ git commit -m 'Greeting from windows 10'
```

提交本地代码到服务器src分支

``` shell
$ git push origin src
```

切换到src分支

``` shell
$ git checkout -q src
```

合并分支(当前处于src分支，要把master分支合并过来)

``` shell
$ git merge master
```

还有其他常用命令暂时没用到或者还没学会 :-(

## 常用Hexo命令

安装git deployer插件

```shell
$ npm install hexo-deployer-git --save
```

清理环境(删除public文件夹)

``` shell
$ hexo clean
```

启动本地服务器

``` shell
$ hexo server
```

部署博客

``` shell
$ hexo g -d
```
