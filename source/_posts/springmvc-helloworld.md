---
title: SpringMVC 的 HelloWorld
date: 2019-01-10 17:42:48
tags:
    - springmvc
    - helloworld
    - 分享

categories: 后端
---


1. 步骤:
- 加入 jar 包
- 在 web.xml 中配置 DispatcherServlet
- 加入 Spring MVC 的配置文件
- 编写处理请求的处理器，并标识为处理器 – 编写视图

2. HelloWorld：加入 jar 包
* jar 包：
    - commons-logging-1.1.3.jar
    - spring-aop-4.0.0.RELEASE.jar
    - spring-beans-4.0.0.RELEASE.jar
    - spring-context-4.0.0.RELEASE.jar
    - spring-core-4.0.0.RELEASE.jar
    - spring-expression-4.0.0.RELEASE.jar – spring-web-4.0.0.RELEASE.jar
    - spring-webmvc-4.0.0.RELEASE.jar
* maven依赖：
```
    <properties>
        <com.yubajin.spring.version>5.0.5.RELEASE</com.yubajin.spring.version>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-expression -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${com.yubajin.spring.version}</version>
        </dependency>
    </dependencies>
```

3. HelloWorld：配置 web.xml	
配置 DispatcherServlet ：DispatcherServlet 默认加载 /WEB-INF/<servletName-servlet>.xml 的 Spring 配置文件, 启动 WEB 层
的 Spring 容器。可以通过 contextConfigLocation 初始化参数自定 义配置文件的位置和名称
![web.xml配置](http://www.yubajin.cn/img/springmvc_helloworld_1.png ''主配置文件配置'')

4. HelloWorld：创建 Spring MVC 配置文件
- 配置自动扫描的包
-  配置视图解析器：视图名称解析器：将视图逻辑 名解析为: /WEB-INF/pages/<viewName>.jsp
默认的配置文件为:/WEB-INF/<servlet-name>-servlet.xml,如springDispatcherServlet-servlet.xml
![servlet.xml配置](http://www.yubajin.cn/img/springmvc_helloworld_2.png ''试图解析器配置'')

5. HelloWorld：创建请求处理器类
![处理器](http://www.yubajin.cn/img/springmvc_helloworld_3.png ''处理器'')
