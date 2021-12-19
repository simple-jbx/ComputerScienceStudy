# Spring

## Spring框架

开源JavaEE的应用程序

核心是IOC（控制反转/依赖注入）和AOP（面向切面编程）

Spring IOC 

Spring AOP

Spring JDBC + 事务

## Spring作用

各层对应的框架

- DAO层 JDBC操作 对应框架：Mybatis、Mybatis-plus
- Service层 Spring框架不是针对service层的业务逻辑的 service目前没有适合的框架
- Controller层 Servlet（接受请求 响应数据 地址配置 页面转发） 对应框架：Spring MVC

Spring基于分布式的应用程序

​		轻量级框架、配置管理、Bean对象的实例化（IOC）

集成第三方的框架

​		Mybatis、Hibernate框架（持久层框架）

​		Spring MVC

​		Spring Security权限

​		Quartz时钟框架（定时任务处理）

自带服务

​		mail邮件发送

​		定时任务处理-定时调度（定时短信、定时任务）

消息处理（异步处理）

## Spring模块划分

Spring IOC模块：Bean对象的实例化 Bean的创建

Spring AOP模块：动态代理 面向切面编程

Spring JDBC+事务模块

Spring web 模块

# Spring IOC

## 主要内容

<div align='center'>
    <img src='./imgs/SpringIOC001.png' width='800px'>
    SpringIOC 主要内容
</div>

