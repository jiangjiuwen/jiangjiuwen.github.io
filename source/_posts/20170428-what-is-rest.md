---
title: 我所理解的REST
categories: 编程之美
date: 2017-04-28 20:39:07
tags:
- REST
- RESTful
keywords: REST,RESTful
---
很多次在浏览技术社区的时候都会看到介绍REST或RESTful的文章，但我一次都没有点开过，所以我对这个术语感到既熟悉又陌生。今天又看到了一篇介绍REST的文章，一时没忍住就点开了，读完之后决定好好了解一下，几番Bing，收获如下。

# 什么是REST

REST是`REpresentational State Transfer`的缩写，中文名又叫`表现层状态转移`，好吧，并不是很懂。搜索了一下才发现这个术语的主语Resource被省略了，REST的全称实际上叫做`Resource Representational State Transfer` ←_←

- Resource：网络资源。比如一个文本，一个视频，一幅图片等实体。
- Representational：表现形式。比如XML、Binaray、Json、PNG等格式。
- State Transfer：状态转移。即资源的状态发生改变，比如被修改或者被删除。

所以，合起来就可以理解为资源在网络中通过某种表现形式进行状态的改变。用知乎大神[Ivony
](https://www.zhihu.com/question/28557115/answer/41265890)的话说就是：用URL定位资源，用HTTP动词(GET、POST、DELETE、PUT)描述操作。

<!-- more -->
## HTTP动词

资源的状态转移，就是通过HTTP动词来描述的，即GET、POST、PUT、DELETE

- GET(SELECT)：从服务器取获取资源（一项或多项）
- POST(CREATE)：在服务器新建一个资源
- PUT(UPDATE)：在服务器更新资源（客户端提供改变后的完整资源）
- PATCH(UPDATE)：在服务器更新资源（客户端提供改变的属性）
- DELETE(DELETE)：从服务器删除资源

## REST和RESTful API的关系

REST是一种抽象的设计风格，他定义了一些接口设计原则，即只要设计接口的时候遵循REST原则，就可以说这个接口具有REST风格。而RESTful API则是指按照REST设计原则实现的具体的网络接口。

# RESTful架构的优势

早期的WEB都是前后端杂糅在一起，但是随着互联网的发展以及各种类型的终端的诞生，以前的设计风格很难适应所有的终端的要求。RESTful的优势就在于它可以在Server端制定一套统一的接口，来为所有的终端提供服务。

一个典型的RESTful架构如下图所示。

![RESTful架构](http://7xr526.com1.z0.glb.clouddn.com/restfull.jpg)

# RESTful架构设计原则

如何设计一个优秀的RESTful架构是一件十分困难的事情，不是看几篇文章就能实现的，需要经过长期的经验积累，由于我还并没有相关经验，这部分内容就暂且按下不表了。
