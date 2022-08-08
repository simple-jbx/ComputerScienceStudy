# MyBatis-Plus简介

MyBatis-Plus（简称 MP）是一个MyBatis的增强工具，在MyBatis的基础上只做增强不做改变，为简化开发、提高效率而生。

## 特性

- 无侵入：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑 
- 损耗小：启动即会自动注入基本CURD，性能基本无损耗，直接面向对象操作 
- 强大的CRUD 操作：内置通用 Mapper、通用Service，仅仅通过少量配置即可实现单表大部分CRUD 操作，更有强大的条件构造器，满足各类使用需求
- 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错 
- 支持主键自动生成：支持多达4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由 配置，完美解决主键问题 
- 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作 
- 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ） 
- 内置代码生成器：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用 
- 内置分页插件：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询 
- 分页插件支持多种数据库：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、 Postgre、SQLServer 等多种数据库 
- 内置性能分析插件：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询 
- 内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 支持数据库

- MySQL，Oracle，DB2，H2，HSQL，SQLite，PostgreSQL，SQLServer，Phoenix，Gauss ， ClickHouse，Sybase，OceanBase，Firebird，Cubrid，Goldilocks，csiidb 
- 达梦数据库，虚谷数据库，人大金仓数据库，南大通用(华库)数据库，南大通用数据库，神通数据库，瀚高数据库

## 框架结构

<div align='center'>
    <img src='./imgs/MyBatisPlus/Springboot/003.png' width='600px'>
</div>


## 代码及文档地址

- 官方地址: http://mp.baomidou.com 
- 代码发布地址: Github: https://github.com/baomidou/mybatis-plus 
- Gitee: https://gitee.com/baomidou/mybatis-plus 
- 文档发布地址: https://baomidou.com/pages/24112f

# 基本CRUD

[代码路径](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study)

```yml
# 配置MyBatis日志
mybatis-plus:
	configuration:
		log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## [通用Mapper](https://github.com/simple-jbx/mybatis-plus/blob/3.0/mybatis-plus-core/src/main/java/com/baomidou/mybatisplus/core/mapper/BaseMapper.java)

[测试Code](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/MybatisPlusSpringbootApplicationTests.java)

## [通用Service](https://github.com/simple-jbx/mybatis-plus/blob/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/service/IService.java)

对Mapper的扩充

说明:

- 通用 Service CRUD 封装[IService (opens new window)](https://gitee.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/service/IService.java)接口，进一步封装 CRUD 采用 `get 查询单行` `remove 删除` `list 查询集合` `page 分页` 前缀命名方式区分 `Mapper` 层避免混淆，
- 泛型 `T` 为任意实体对象
- 建议如果存在自定义通用 Service 方法的可能，请创建自己的 `IBaseService` 继承 `Mybatis-Plus` 提供的基类
- 对象 `Wrapper` 为 [条件构造器](https://baomidou.com/01.指南/02.核心功能/wrapper.html)

[测试Code](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/MybatisPlusServiceTest.java)



# 常用注解

## @TableName

### 问题

MyBatis-Plus在确定操作的表时，由BaseMapper的泛型决定，即实体类型决定，且默认操作的表名和实体类型的类名一致，若表名和实体类型类名不一致则会抛出异常。

### 解决

#### 通过@TableName配置

在实体类类型上添加@TableName("t_name")，标识实体类对应的表，即可成功执行SQL语句。

#### 通过全局配置解决问题

实体类所对应的表都有固定的前缀，例如`t_`或`tbl_`，此时，可以使用MyBatis-Plus提供的全局配置，为实体类所对应的表名设置默认的前缀，那么就不需要在每个实体类上通过@TableName标识实体类对应的表。

```yml
mybatis-plus:
	configuration:
		# 配置MyBatis日志
		log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
	global-config:
		db-config:
		# 配置MyBatis-Plus操作表的默认前缀
			table-prefix: t_
```

## @TableId

MyBatis-Plus在实现CRUD时，会默认将id作为主键列，并在插入数据时，默认基于雪花算法的策略生成id。

### 问题

若实体类和表中表示主键的不是id，而是其他字段，例如uid，MyBatis-Plus则不能识别，会抛出异常。

### 解决

#### 通过@TableId解决。

```java
@TableId
private Long uid;
```

#### @TableId的value属性

若实体类中主键对应的属性为id，而表中表示主键的字段为uid，此时若只在属性id上添加注解 @TableId，则抛出异常Unknown column 'id' in 'field list'，即MyBatis-Plus仍然会将id作为表的 主键操作，而表中表示主键的是字段uid。

此时需要通过@TableId注解的value属性，指定表中的主键字段，@TableId("uid")或 @TableId(value="uid")。

#### @TableId的type属性

type属性用来定义主键策略

**常用主键策略**

| 值                       | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| IdType.ASSIGN_ID（默认） | 基于雪花算法的策略生成数据id，与数据库id是否设置自增无关。   |
| IdType.AUTO              | 使用数据库的自增策略，注意，该类型请确保数据库设置了id自增，否则无效。 |

```yml
#配置全局主键策略
mybatis-plus:
	configuration:
		# 配置MyBatis日志
		log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
	global-config:
		db-config:
			# 配置MyBatis-Plus操作表的默认前缀
			table-prefix: t_
			# 配置MyBatis-Plus的主键策略
			id-type: auto
	# 配置类型别名所对应的包
  	type-aliases-package: tech.snnukf.mybatisplusspringboot.entity
  	# 扫描通用枚举的包
  	type-enums-package: tech.snnukf.mybatisplusspringboot.enum
```

#### 雪花算法

在实际的业务中需要选择合适的方案去应对数据规模的增长，以应对逐渐增长的访问压力和数据量。 数据库的扩展方式主要包括：业务分库、主从复制，数据库分表。

**数据库分表**

单表数据拆分有两种方式：垂直分表和水平分表。

<div align='center'>
    <img src='./imgs/MybatisPlus/Springboot/001.png' width='600px'>
</div>

- 垂直分表
  - 垂直分表适合将表中某些不常用且占了大量空间的列拆分出去。
- 水平分表
  - 水平分表适合表行数特别大的表。水平分表相比垂直分表，会引入更多的复杂性，例如要求全局唯一的数据id该如何处理。
  - 主键自增
    - 复杂点：分段大小的选取。分段太小会导致切分后子表数量过多，增加维护复杂度；分段太大可能会 导致单表依然存在性能问题，一般建议分段大小在 100 万至 2000 万之间，具体需要根据业务选取合适 的分段大小。
    - 优点：可以随着数据的增加平滑地扩充新的表。例如，现在的用户是 100 万，如果增加到 1000 万， 只需要增加新的表就可以了，原有的数据不需要动。
    - 缺点：分布不均匀。假如按照 1000 万来进行分表，有可能某个分段实际存储的数据量只有 1 条，而 另外一个分段实际存储的数据量有 1000 万条。
  - 取模
    - 复杂点：初始表数量的确定。表数量太多维护比较麻烦，表数量太少又可能导致单表性能存在问题。
    - 优点：表分布比较均匀
    - 缺点：扩充新的表很麻烦，所有数据都要重分布
  - 雪花算法
    - 由Twitter公布的分布式主键生成算法，它能够保证不同表的主键的不重复性，以及相同表的 主键的有序性。
    - 核心思想：长度共64bit（一个long型）。首先是一个符号位，1bit标识，由于long基本类型在Java中是带符号的，最高位是符号位，正数是0，负 数是1，所以id一般是正数，最高位是0。41bit时间截(毫秒级)，存储的是时间截的差值（当前时间截 - 开始时间截)，结果约等于69.73年。 10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID，可以部署在1024个节点）。 12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID）。
    - 优点：整体上按照时间自增排序，并且整个分布式系统内不会产生ID碰撞，并且效率较高。

## @TableField

MyBatis-Plus在执行SQL语句时，要保证实体类中的属性名和 表中的字段名一致。如果实体类中的属性名和字段名不一致则需要使用这个注解。

- 情况1

  - 若实体类中的属性使用的是驼峰命名风格，而表中的字段使用的是下划线命名风格
  - 例如实体类属性userName，表中字段user_name，则会自动转换

- 情况2

  - 若实体类中的属性和表中的字段不满足情况1则需要使用该注解

  - ```java
    @TableField("username")
    private String name;
    ```

## @TableLogic

表明逻辑删除属性字段用于逻辑删除。

```java
@TableLogic
private Integer isDeleted;
```

# 条件构造器和常用接口

## wapper介绍

<div align='center'>
    <img src='./imgs/MybatisPlus/Springboot/002.png' width='600px'>
</div>

- Wrapper ： 条件构造抽象类，最顶端父类 
  - AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件 
    - QueryWrapper ： 查询条件封装 
    - UpdateWrapper ： Update 条件封装 
    - AbstractLambdaWrapper ： 使用Lambda 语法 
      - LambdaQueryWrapper ：用于Lambda语法使用的查询Wrapper 
      - LambdaUpdateWrapper ： Lambda 更新封装Wrapper

## [QueryWrapper](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/MybatisPlusWrapperTest.java)

###  组装查询条件

```java
    queryWrapper
        .like("name", "bo")
        .between("age", 20, 25)
        .isNotNull("email");
    List<User> list = userMapper.selectList(queryWrapper);
```

### 组装排序条件

```java
    queryWrapper
        .orderByDesc("age")
        .orderByAsc("id");
    }
```

### 组装删除条件

```java
	QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper.isNull("email");
    //条件构造器也可以构建删除语句的条件
    int result = userMapper.delete(queryWrapper);
```

### 组装修改条件

```java
		queryWrapper
                .gt("age", 20)
                .like("name", "a")
                .or()
                .isNull("email");

        queryWrapper
                .like("name", "a")
                .and(i -> i.gt("age", 20).or().isNull("email"));
```

### 组装select语句

```java
		queryWrapper.select("name", "age");
        List<Map<String, Object>> maps = userMapper.selectMaps(queryWrapper);
```

###  实现子查询

```java
queryWrapper.inSql("id", "select id from user where id <= 5");
```

## [UpdateWrapper](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/MybatisPlusWrapperTest.java)

```java
        updateWrapper
                .set("age", 18)
                .set("email", "user@snnukf.tech")
                .like("name", "a")
                .and(i -> i.gt("age", 20).or().isNull("email"));
```

## Condition

在实际开发中，组装的条件来源于用户输入（可选），需要对这些条件进行判断，选择性组装，以免影响到SQL执行的结果。

### 方法一

可以使用多个if-else进行判断来组装查询语句，编码较为复杂，不利于后期维护。

### 方法二

使用带有condition参数的重载方法构建查询语句。

```java
    public Children like(boolean condition, R column, Object val) {
        return likeValue(condition, LIKE, column, val, SqlLike.DEFAULT);
    }
	
	//如果condition为false则跳过该条件
	queryWrapper
                .like(StringUtils.isNotBlank(name), "name", name)
                .ge(ageBegin != null, "age", ageBegin)
                .le(ageEnd != null, "age", ageEnd);
```



## LambdaQueryWrapper

好处是可以不用手动敲字段名称，防止出错。

```java
 LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        //避免使用字符串表示字段，防止运行时错误
        //SELECT id,name,age,email FROM user WHERE (name LIKE ? AND age >= ? AND age <= ?)
        queryWrapper
                .like(StringUtils.isNotBlank(username), User::getName, username)
                .ge(ageBegin != null, User::getAge, ageBegin)
                .le(ageEnd != null, User::getAge, ageEnd);
```

## LambdaUpdateWrapper

```java
 LambdaUpdateWrapper<User> updateWrapper = new LambdaUpdateWrapper<>();
        //UPDATE user SET age=?,email=? WHERE (name LIKE ? AND (age < ? OR email IS NULL))
        updateWrapper
                .set(User::getAge, 18)
                .set(User::getEmail, "user@snnukf.tech")
                .like(User::getName, "a")
                .and(i -> i.lt(User::getAge, 24).or().isNull(User::getEmail));
```

# 插件

## 分页插件

MyBatis Plus自带分页插件，只要简单的配置即可实现分页功能

### 添加配置类

```java
@Configuration
@MapperScan("tech.snnukf.mybatisplusspringboot.mapper")
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new
                PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}
```

### [测试Code](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/MybatisPlusPageTest.java)

```java
//SELECT COUNT(*) AS total FROM user
        //SELECT id,name,age,email FROM user LIMIT ?,?
        Page<User> page = new Page<>(2, 5);
        userMapper.selectPage(page, null);
        //获取分页数据
        List<User> list = page.getRecords();
```

## xml自定义分页

###   UserMapper中定义接口方法

```java
    /**
     * 通过年龄查询用户信息并分页
     * @param page MyBatis-Plus所提供的分页对象，必须位于第一个参数的位置
     * @param age
     * @return
     */
    Page<User> selectPageVo(@Param("page") Page<User> page, @Param("age") Integer age);
```

### UserMapper.xml中编写SQL

```xml
<!--SQL片段，记录基础字段-->
<sql id="BaseColumns">id,username,age,email</sql>
<!--IPage<User> selectPageVo(Page<User> page, Integer age);-->
<select id="selectPageVo" resultType="User">
SELECT <include refid="BaseColumns"></include> FROM t_user WHERE age > #
{age}
</select>
```

## 乐观锁

### 乐观锁与悲观锁的区别

```java
//TODO
```

### 乐观锁实现流程

数据库中设置一个version字段，更新前先获取version_old，更新时version = version_old+1，如果插入数据时version_old与数据库中的version不一致则说明数据被修改了，则更新失败。

```sql
UPDATE product SET price=price+50, `version`=`version_old` + 1 WHERE id=1 AND `version`=version_old
```

### Mybatis-Plus实现乐观锁

```java
        //添加乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
```

```java
	//实体类添加注解
	@Version
	private Integer version;
```

# 通用枚举

## [创建通用枚举类型](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/tree/main/src/main/java/tech/snnukf/mybatisplusspringboot/enums/SexEnum.java)

## [配置扫描通用枚举](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/main/resources/application.yml)

# 代码生成器

## 引入依赖

```xml
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.1</version>
        </dependency>
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
        </dependency>
```

[Code](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/blob/main/src/test/java/tech/snnukf/mybatisplusspringboot/FastAutoGeneratorTest.java)

[代码生成文件](https://github.com/simple-jbx/Mybatis-Plus-Springboot-Study/tree/main/mybatis-plus-auto-generator)

# 多数据源

适用于多种场景：纯粹多库、 读写分离、 一主多从、 混合模式等。

# MyBatisX插件

MyBatisX插件用法：https://baomidou.com/pages/ba5b24/

# Reference

- [学习视频地址](https://www.bilibili.com/video/BV1VP4y1c7j7)

- 官方地址: http://mp.baomidou.com 
- 代码发布地址: Github: https://github.com/baomidou/mybatis-plus 
- Gitee: https://gitee.com/baomidou/mybatis-plus 
- 文档发布地址: https://baomidou.com/pages/24112f
