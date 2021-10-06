# 注解

[参考链接](https://www.jianshu.com/p/f457a575e155)

## @Mapper

作用：在接口类添加@Mapper，在编译后会生成响应的接口实现类

添加位置：接口类上面

## @MapperScan

作用：指定要编程实现类的接口所在的包，然后包下面的所有接口在编译之后都会生成响应的实现类

添加位置：在Springboot启动类上面添加

```java
package tech.snnukf.seckillsys;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("tech.snnukf.seckillsys.pojo")
public class SecKillSysApplication {
    public static void main(String[] args) {
        SpringApplication.run(SecKillSysApplication.class, args);
    }
}

```

## @Controller

用于标注是控制层组件，需要返回页面时使用

## @RequestMapping

处理请求地址映射的注解，可用于类或者方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父地址。

- params：指定request中必须包含某些参数值时，才让该方法处理。
- headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。
- value：指定请求的实际地址，指定的地址可以是URI Template模式
- method：指定请求的method类型，GET、POST、PUT、DELETE等
- consumes：指定处理请求的提交内容类型（Content-Type），如application/json,text/html
- produces：指定返回的内容类型，仅当request请求头中的（Accept）类型中包含指定类型才返回

## @RestController