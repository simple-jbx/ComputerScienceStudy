# [秒杀系统](https://github.com/simple-jbx/SecKill/blob/main/seckill.md)



## 技术点与实施方案

<div align='center'>
    <img src='./imgs/seckill.svg' width='1600px'>
	</br></br>秒杀系统技术点与方案
</div>




### 技术点总结

#### 前端

- Thymeleaf
	- 与html搭配实现动态页面（类似html+jsp）
	- 使用简单，只需创建Controller然后在resources/templates下创建对应的html即可
	- Thymeleaf页面跳转需要通过Controller来跳转，不能直接通过html，否则会出错
- Bootstrap
- Jqueryy

#### 后端

- SpringBoot

- MybaitsPlus（基于Mybaits封装）

- Lombok（注解）

#### 中间件

- RabbitMQ（异步 流量均分）

- Redis（缓存、数据库、消息中间件）

### 秒杀方案

- 功能开发
	- 商品列表
	- 商品详情
	- 秒杀
	- 订单详情
- 分布式Session
	- 用户登陆
	- 共享Session
- 压力测试
	- JMeter入门
	- 自定义变量
	- 正式压测
- 页面优化
	- 缓存
	- 静态化分离
- 服务优化
	- RabbitMQ消息队列
	- 接口优化
	- 分布式锁
- 接口安全
	- 隐藏秒杀地址
	- 验证码
	- 接口限流

### 如何设计一个秒杀系统

稳（高性能）、准（一致性）、快（高可用）

- 高性能。涉及大量并发读、写，支持高并发访问非常关键。

- 一致性。商品库存的实现方式同样关键。

- 高可用。难免会遇到考虑不到的情况，要保证系统的高可用性和准确性。