# 注解

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

