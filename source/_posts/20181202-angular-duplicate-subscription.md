---
title: Angular重复订阅的问题
categories: 编程之美
date: 2018-12-02 17:18:13
tags:
- Angular
---
在Angular中使用可观察对象进行发布和订阅的时候容易造成多次订阅的情况，比如我们通常在OnInit生命周期函数中进行可观察对象的订阅，由于可观察对象是一个单例，当我们下次实例化当前类的时候，又会订阅一次该可观察对象，从而导致数据发布更新的时候，订阅者会多次响应订阅操作。

实际上当我们订阅该对象的时候会返回一个观察者对象，而造成重复订阅的根本原因是因为没有在合适的时机将该观察者取消订阅。通常在销毁该类的时候取消订阅是一个不错的选择，即OnDestroy生命周期函数。

```TypeScript
// 声明观察者对象
saveDataObserver: Subscription;

OnInit() {
    // 订阅可观察对象，返回观察者
    this.saveDataObserver = this.svc.saveData$.subscript(data => {...});
}

OnDestroy() {
    // 取消订阅，防止重复订阅
    this.saveDataObserver.unsubscript();
}
```
