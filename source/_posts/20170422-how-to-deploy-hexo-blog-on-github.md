---
title: 如何在Github上构建并部署Hexo博客
categories: Web
date: 2017-04-22 20:45:12
tags:
- Hexo
- GitHub
---
如果你在网上搜索Hexo GitHub关键字，会出现无数教你利用GitHub和Hexo构建一个免费的个人博客的教程，所以本文不再赘述，本文旨在记录我这两天搭建博客的过程中遇到的问题以及解决办法。

# 环境配置

首先当然是安装Node.js，Hexo以及Git(因为要部署博客和托管源码到GitHub)，几条命令就解决了，轻松加愉快。

以下是Hexo官网提供的几条最基本的使用命令

``` shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

值得注意的是，如果是从远程仓库Pull的源码，则不需要初始化Hexo文件夹，只需要执行第四行*install*命令就够了，因为远程仓库中的文件是已经初始化过的，如果再次*init*会使自定义配置文件被默认文件覆盖。

# 绑定独立域名

作为一个个人博客，拥有一个独立域名是一件多么炫酷的事情。

## 修改DNS

到域名管理面板增加两条A记录和一条CNAME

> A @ 192.168.255.253
  
> A @ 192.168.255.253
  
> CNAME www jiangjiuwen.github.io

## 创建CNAME文件(无后缀)

该文件里面有且仅有一行你的独立域名，然后将其部署到博客，那么问题来了，当我第二次deploy博客的时候，这个文件不见了，此时独立域名失效了，其实是因为每次deploy都会重新生成新的文件，而CNAME文件是用户手动添加的，所以不会自动生成。

为了解决这个问题，只需要将CNAME文件放入blog/source文件夹下，然后再deploy博客，该文件就会随博客一起部署了，同理，其他由用户手动创建并需要被发布的文件都需要放在该文件夹下。

# 多设备同时部署

通常我们只会使用一台电脑上工作，但是，有时候我们也会遇到需要在另一台电脑上写博客的情况，如果重新配置环境，就无法在当前博客版本上生成的博客文件，这是无法接受的。

因此，我在把博客部署到Repo的master分支的时候，同时会把源代码一并push到另外一个branch上(比如本博客的*src*分支)，这样当我们在另外一台电脑上配置环境的时候只需要pull *src*分支就可以在最新的版本上写博客了，deploy之后将修改内容一并push到分支上，这样回到主要的工作电脑上就可以将修改pull到本地继续工作。

# 备注

搭建博客的过程中学到了很多命令，包括git命令和hexo命令，避免忘记，在此记录一下

## 常用Git命令

连接远程服务器

``` shell
$ git remote add origin git@github.com:jiangjiuwen/jiangjiuwen.github.io.git
```

从服务器获取src分支代码

``` shell
$ git pull origin src
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

还有其他常用命令暂时没用到或者还没学会 ……

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
