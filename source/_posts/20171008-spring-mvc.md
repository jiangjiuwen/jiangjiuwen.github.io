---
title: Spring MVC
categories: 编程之美
date: 2017-10-08 15:16:24
tags:
- Spring MVC
- JAVA
---
Spring是当前比较流行的一种WEB框架，通过实现Model-View-Controller模式来很好地将视图和模型分离。SpringMVC是一个基于DispatcherServlet的MVC框架，每一个请求最先访问的都是DispatcherServlet，DispatcherServlet负责转发每一个Request请求给相应的Handler，Handler处理以后再返回相应的视图(View)和模型(Model)，返回的视图和模型都可以不指定，即可以只返回Model或只返回View或都不返回。

# Spring MVC模式
## 工作流
- 前端发出一个请求。
- DispatcherServlet将该请求转发给相应的控制器。
- 控制器对请求进行响应，处理完成之后，控制器会将处理结果委派给一个视图或模型。
- 控制器将视图或模型返回给DispatcherServlet，它会根据一个视图解析器将视图的名称解析为一个具体的视图实现。这个视图解析器是一个实现了ViewResolver接口的Bean，它的任务就是返回一个视图的具体实现。

## 常用注解
@Controller：控制器类注解。当控制器类接收到一个请求时，它会在自己内部存照一个合适的方法来处理请求。
@RequestMapping：将方法映射到请求上的注解。其实RequestMapping在Class上，可看做是父Request请求url，而RequestMapping在方法上的可看做是子Request请求url，父子请求url最终会拼起来与页面请求url进行匹配。
在SpringMVC中常用的注解还有@PathVariable，@RequestParam，@PathVariable标记在方法的参数上，利用它标记的参数可以利用请求路径传值


# Spring MVC的相关配置
SpringMVC是一个基于DispatcherServlet的，首先配置DispatcherServlet，在`web.xml`中配置DispatcherServlet的name、class以及startup

`spring-servlet.xml`配置，注意，这个名称是由web.xml中的spring的name标签中的值加上`-servlet`组成的，前缀是自定义的。这个里面会配置是否启用spring mvc注解、注解的类所用的jar包，跳转页面的路径解析（前缀、后缀等）等内容。


# 拦截器
编写拦截器极其简单，只要编写一个类，实现HandlerInterceptor的方法，然后在springMVC的配置文件中配置这个类，就可以使用这个拦截器了。比如权限控制等。

在使用拦截器时候，并不一定要对所有的目标方法都进行拦截，所以我们可以只对指定的方法进行拦截，这就需要更改配置文件了，只需要在<mvc:interceptors>中配置一个<mvc:interceptor>，然后指定其路径，就可以了，这个路径可以指定一个URL，也可以使用通配符。

当同时定义了多个拦截器的时候，那么它的使用顺序如下：
- preHandle：当目标方法执行之前，执行此方法，如果返回false，则不会执行目标方法，同样的，后面的拦截器也不会起作用。
- postHandle：执行目标方法之后调用，但是在渲染视图之前，就是转向jsp页面之前，以对请求域中的属性，或者视图进行修改。
- afterCompletion：在渲染视图之后被调用，释放资源。