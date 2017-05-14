---
title: 单例模式
categories: Design pattern
date: 2017-05-08 20:31:56
tags:
- 设计模式
- 单例模式
keywords: '设计模式,单例模式'
---
单利模式属于创建型模式，确保一个类只有一个实例，且提供一个访问它的全局访问点。主要是为了防止一个全局使用的类频繁地创建和销毁，影响性能浪费资源。

使用场景：log系统

单例模式具有如下特征

- 单例类只能有一个实例
- 单例类必须自己创建自己的唯一实例
- 单例类必须给其他对象提供这一实例

``` C#
public class SingletonPattern
{
    private static SingletonPattern _instance;

    private SingletonPattern() { }

    public static SingletonPattern GetInstance()
    {
        if (_instance == null)
        {
            _instance = new SingletonPattern();
        }
        return _instance;
    }

    public void SayHello()
    {
        Console.WriteLine("Hello Jiang");
    }
}
```