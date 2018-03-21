---
title: 在Ubuntu系统中使用不同版本的Python环境
categories: 编程之美
date: 2018-03-21 18:47:00
tags:
- Python
- Ubuntu
- virtualenvwrapper
---
Ubuntu16.4版本中默认包含python2.7和Python3.5两个版本，命令行中可以分别使用python和python3来启动并进入各自环境。

virtualenv用于创建独立的Python环境，使得多个Python相互隔离，互不影响，它能够：
- 在没有权限的情况下安装python包
- 不同应用可以使用不同版本的python包
- 一个环境下的python包升级不影响其他环境

顾名思义，virtualenvwrapper则是对虚拟环境进行管理，是virtualenv的扩展包，它能够：
- 将所有虚拟环境整合在一个目录下
- 管理（新增，删除，复制）虚拟环境
- 切换虚拟环境

# 安装virtualenvwrapper
对于一台全新的Ubuntu电脑，可能还需要首先安装pip，即python包管理器，命令如下。
```bash
sudo apt-get install python-pip  # 对应python2
sudo apt-get install python3-pip  # 对应python3
```
之后使用pip安装virtualenvwrapper。
```bash
sudo pip install virtualenvwrapper
```
注意，系统默认采用python2，所以，如果使用pip3进行安装，之后会报错，此处按下不表。

# 配置virtualenvwrapper
virtualenvwrapper安装完成之后需要对其进行简单配置。
```bash
mkdir $HOME/.venvs
sudo vim ~/.bashrc

# 在打开的.bashrc文件中添加如下2行：
export WORKON_HOME=$HOME/.venvs
source /usr/local/bin/virtualenvwrapper.sh
```

如果virtualenvwrapper是使用pip3进行安装，则会报如下错误。

```bash
/usr/bin/python: No module named virtualenvwrapper
virtualenvwrapper.sh: There was a problem running the initialization hooks.

If Python could not import the module virtualenvwrapper.hook_loader,
check that virtualenvwrapper has been installed for
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python and that PATH is
set properly.
```

至此，virtualenvwrapper的安装和配置告一段落。

# 使用virtualenvwrapper管理虚拟环境
基于不同的python版本创建虚拟环境
```bash
mkvirtualenv -p /usr/bin/python2 scrapy_py2  # 创建基于python2的虚拟环境scrapy_py2
mkvirtualenv -p /usr/bin/python3 scrapy_py3  # 创建基于python3虚拟环境scrapy_py3
# 如果提示mkvirtualenv权限问题，请检查$HOME/.venvs文件所属的用户和组。
```
创建成功之后系统默认进入当前虚拟环境，命令行前缀显示当前虚拟环境名称。进入虚拟环境之后所有的操作都基于当前python环境，不会对其他python环境产生影响。

其他命令如下所示
```bash
workon  # 列出所有的虚拟环境
workon scrapy_py2  # 切换到虚拟环境scrapy_py2
deactivate  # 退出当前虚拟环境
rmvirtualenv scrapy_py2  # 删除虚拟环境scrapy_py2
```
