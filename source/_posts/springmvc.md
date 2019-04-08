---
title: SpringMVC
date: 2019-01-24 16:16:07
tags:
    - springmvc
    - 配置
    - 错误解决
    - 分享

categories: 后端
---
# SpringMVC 概述
- Spring 为展现层提供的基于 MVC 设计理念的优秀的 Web 框架，是目前最主流的 MVC 框架之一
- Spring3.0 后全面超越 Struts2，成为最优秀的 MVC 框架 • Spring MVC 通过一套 MVC 注解，让 POJO 成为处理请求的控制器，而无须实现任何接口。
- 支持 REST 风格的 URL 请求
- 采用了松散耦合可插拔组件结构，比其他 MVC 框架更具扩展性和灵活性

# springMVC 返回中文字符串时乱码解决方法
第一种：在@RequestMapping中添加produces="text/html;charset=UTF-8
快速链接:[csdn博客](https://blog.csdn.net/yaov_yy/article/details/51819567)

# 传递对象作为参数（POJO）,springmvc自动将前台传过来的参数一个一个封装到对象中
快速链接:[csdn博客](https://blog.csdn.net/u010837612/article/details/45199919)

# springmvc支持跨域请求配置，在filter中配置

```
httpServletResponse.setHeader("Access-Control-Allow-Origin", "*");
httpServletResponse.setHeader("Access-Control-Allow-Headers", "Authentication");
```

原理:[阮一峰博客](http://www.ruanyifeng.com/blog/2016/04/cors.html)
实践:
1. 写个filter
![filter](http://www.yubajin.cn/img/springmvc_readfilter.png ''过滤器'')
2. 配置在web.xml
![web.xml配置详解](http://www.yubajin.cn/img/springmvc_webxml.png ''web.xml配置'')
[详情请查看](https://blog.csdn.net/jinzhencs/article/details/51538274)
