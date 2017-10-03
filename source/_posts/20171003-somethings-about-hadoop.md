---
title: Somethings about Hadoop
categories: Big Data
date: 2017-10-03 15:56:12
tags:
- Hadoop
- Big data
---

# 前言
谷歌大数据技术MapReduce、BigTable、GFS突破了存储容量、读写速率、计算效率的瓶颈，带来了革命性的变化：
- 成本降低。能用家用PC计算机。
- 软件容错硬件故障视为常态，通过软件保证高可靠性。
- 简化并行分布式计算，无须控制节点同步和数据交换。

但是谷歌只发表了论文，没有公开源码，所以开发者模仿Google大数据技术，创造了一个开源的分布式存储和分布式计算平台`Hadoop`。

Hadoop包括两个核心组成：
- HDFS。分布式文件系统，实现海量数据的存储。
- MapReduce。并行处理框架，实现任务的分解和调度。

Hadoop优势：高扩展、低成本、成熟的生态圈

常见的Hadoop工具包括：HIVE、HBASE、Zookeeper


# Hadoop的安装

1. 安装jdk设置环境变量
2. 安装hadoop应用程序
3. 配置hadoop运行环境

```
hadoop-env.sh 配置java_home
```
```
core-site.xml 
- hadoop.tmp.dir hadoop的临时工作目录
- dfs.name.dir 所有元数据的目录
- fs.default.name 文件系统namenode访问地址
```
```
hdfs-site.xml
- dfs.data.dir 文件系统的数据存放目录
```
```
mapred-site.xml
- mapred.job.tracker 任务调度器该如何去访问
```

最后对Hadoop的NameNode进行一个格式化操作

JPS查看当前运行的进程，查看Hadoop是否正常运行

```
- JobTracker
- DataNode
- TaskTracker
- NameNode
- SecondaryNameNode
```
<!-- more -->

# HDFS
## HDFS的设计架构
- 块（block）。HDFS的文件被分成块进行存储，默认大小64M，块是文件存储处理的逻辑单元。
- NameNode。管理节点，存放文件元数据，元数据包括：
    - 文件与数据块的映射表
    - 数据节点与数据块的映射表
- DataNode。DataNode是HDFS的工作节点，存放具体的数据块。

## HDFS中数据管理策略
- 数据块的放置：每个数据块3个副本，分布在两个机架内的三个节点上，保证硬件上的容错。
- 心跳检测：DataNode定期向NameNode发送心跳消息。
- 二级NameNode：定期同步元数据映射文件和修改日志，NameNode发生故障时，使用SecondaryNameNode


## HDFS中文件读取流程
读文件
- 客户端向NanmeNode发起请求（文件名，路径），NameNode返回客户端，这个文件包含哪些块，在哪些DataNode；
- 客户端直接去读取Block，进行组装。

写文件
- 客户端将文件拆分成块（block），之后通知NameNode；
- NameNode找到一些可用的DataNode并返回给客户端；
- 客户端根据这些块信息进行写入Block；
- 写入一个块之后要进行流水线复制，即写完一个block之后通过一个复制管道复制到另一个DataNode，然后再复制到另外一个机器上的DataNode；
- 复制完成之后，更新NameNode的元数据；
- 写完一个block之后继续以上步骤写入下一个block。

## HDFS的特点
- 数据冗余，硬件容错；
- 流式是的数据访问；
- 适合存储大文件，小文件会使NameNode压力增大；
- 适合数据批量读写，吞吐量高，不适合做交互式应用，低延迟很难满足；
- 适合一次写入，多次读取，顺序读写，不支持多用户并发写相同文件。


# MapReduce
## MapReduce的原理
MapReduce的本质就是分而治之，就是将一个大任务拆分成多个小的子任务(Map)，多个任务节点进行并行执行之后，合并结果(Reduce)。

比如，通过日志分析，找到用户最常使用的功能。将日志按照时间进行拆分（Map过程），得到每个功能触发的次数，然后将每个任务的结果交换并进行合并（Reduce过程），排序之后得到总的功能使用统计情况。


# 总结
大数据存储与处理技术的原理。
什么是Hadoop？
Hadoop的基本思想？
如何安装Hadoop？
Hadoop的重要组成部分：HDFS、MapReduce。
Hadoop的应用实例。