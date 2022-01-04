# Spring

[代码地址](https://github.com/simple-jbx/SpringLearning)

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

​		Quartz时钟框架，定时任务处理-定时调度（定时短信、定时任务）

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
## Spring IOC容器Bean对象实例化模拟

这部分代码在Spring02目录下

思路：

1. 定义Bean工厂接口，提供获取bean方法
2. 定义Bean工厂接口实现类，解析配置文件，实例化Bean对象
3. 实现获取Bean方法

### 定义 Bean 属性对象

```java
package tech.snnukf.spring;

/**
 * @author simple.jbx
 * @ClassName MyBean
 * @description Bean 属性对象
 * 用来存储配置文件中bean标签对应的id和class属性值
 * @email jb.xue@qq.com
 * @github https://github.com/simple-jbx
 * @date 2021/12/30/ 19:07
 */
public class MyBean {
    private String id;
    private String clazz;

    public MyBean(String id, String clazz) {
        this.id = id;
        this.clazz = clazz;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getClazz() {
        return clazz;
    }

    public void setClazz(String clazz) {
        this.clazz = clazz;
    }
}
```

### 添加 dom4j 依赖

```xml
  <dependencies>
    <dependency>
      <groupId>dom4j</groupId>
      <artifactId>dom4j</artifactId>
      <version>1.6.1</version>
    </dependency>

    <dependency>
      <groupId>jaxen</groupId>
      <artifactId>jaxen</artifactId>
      <version>1.1.1</version>
    </dependency>
```

### 准备自定义配置文件

````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
    <bean id="userDao" class="tech.snnukf.dao.UserDao"></bean>
    <bean id="userService" class="tech.snnukf.service.UserService"></bean>
</beans>
````

### 定义 Bean 工厂接口

```java
package tech.snnukf.spring;

/**
 * @author simple.jbx
 * @className MyFactory
 * @description 工厂接口
 * @email jb.xue@qq.com
 * @github https://github.com/simple-jbx
 * @date 2021/12/30/ 19:21
 */
public interface MyFactory {
    
    /**
     * @author simple.jbx
     * @description 通过id属性值获取对象
     * @date 19:22 2021/12/30
     * @param	id
     * @return 
     **/
    public Object getBean(String id);
}
```

### 定义 Bean 接口的实现类

```java
package tech.snnukf.spring;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.XPath;
import org.dom4j.io.SAXReader;

import java.net.URL;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author simple.jbx
 * @ClassName 模拟Spring实现
 * 1. 通过带参构造器得到对应的配置文件
 * 2. 通过dom4j解析配置文件（xml文件），
 * 得到List集合（存放bean标签的id和class属性值）
 * 3. 通过反射得到对应的实例化对象 遍历List集合获取对应class属性，利用
 * Class.forName(class).newInstance()
 * 4. 通过id属性值获取指定的实例化对象
 * @description //TODO
 * @email jb.xue@qq.com
 * @github https://github.com/simple-jbx
 * @date 2021/12/30/ 19:22
 */
public class MyClassPathXmalApplicationContext implements MyFactory{

    /*存放从配置文件中获取到的bean标签信息（MyBean代表的是每一个bean标签）*/
    private List<MyBean> beanList;
    private Map<String, Object> beanMap = new HashMap<>();

    public MyClassPathXmalApplicationContext(String fileName) {

        /*通过dom4j解析配置文件（xml文件），得到List集合*/
        this.parseXml(fileName);

        /*通过反射得到对应的实例化对象，放置在Map对象*/
        this.instanceBean();
    }

    /**
     * @author simple.jbx
     * @description 通过反射得到对应的实例化队形，防止在Map中
     * 1. 判断对象集合是否为空，如果不为空则遍历集合，获取对象id和class属性
     * 2. 通过类的全路径名 反射得到实例化对象， Class.forName(class).newInstance()
     * 3. 将对应的id和实例化好的bean对象设置到Map对象中
     * @date 20:56 2021/12/30
     * @param
     * @return
     **/
    private void instanceBean() {
        //1.  判断对象集合是否为空，如果不为空，则遍历集合，获取对象的id和class属性
        if(beanList != null && beanList.size() > 0) {
            for (MyBean bean : beanList) {
                String id = bean.getId();
                String clazz = bean.getClazz();
                try {
                    // 2.通过类的全路径名 反射得到实例化对象
                    Object object = Class.forName(clazz).newInstance();
                    // 3.将对应的id和实例化好的bean对象设置到map对象中
                    beanMap.put(id, object);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * @author simple.jbx
     * @description 通过dom4j解析配置文件，得到List集合
     * 1. 获取解析器
     * 2. 获取配置文件的URL
     * 3. 通过解析器解析配置文件（xml文件）
     * 4. 通过xpath语法解析，获取beans标签下的所有bean标签
     * 5. 通过指定解析语法解析文档对象，返回元素集合
     * 6. 判断元素集合是否为空
     * 7. 集合不为空，遍历集合
     * 8. 获取bean标签元素的属性（id、class属性）
     * 9. 获取MyBean对象，将id和class属性值设置到对象中，再将对象设置到MyBean的集合中
     * @date 20:41 2021/12/30
     * @param	fileName
     * @return
     **/
    private void parseXml(String fileName) {
        //1.获取解析器
        SAXReader saxReader = new SAXReader();
        //2.获取配置文件的URL
        URL url = this.getClass().getClassLoader().getResource(fileName);
        System.out.println(url);
        try {
            //3. 通过解析器配置文件
            Document document = saxReader.read(url);
            //4. 通过xpath语法解析，获取beans标签下的所有bean标签
            XPath xPath = document.createXPath("beans/bean");
            //5. 通过制定的解析语法解析文档对象，返回元素集合
            List<Element> elementList = xPath.selectNodes(document);
            if(elementList != null && elementList.size() > 0) {
                beanList = new ArrayList<>();

                //7. 如果集合不为空，遍历集合
                for (Element el : elementList) {
                    //8. 获取bean标签元素的属性（id、class）
                    String id = el.attributeValue("id");
                    String clazz = el.attributeValue("class");
                    //获取MyBean对象，将id和class属性值设置到对象中，在将对象设置到MyBean中
                    MyBean myBean = new MyBean(id, clazz);
                    beanList.add(myBean);
                }
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * @author simple.jbx
     * @description 通过id获取对应map中的value（实例化的bean对象）
     * @date 20:40 2021/12/30
     * @param	id
     * @return
     **/
    @Override
    public Object getBean(String id) {
        return beanMap.get(id);
    }
}
```

### 测试自定义 IOC 容器

```java
package tech.snnukf;

import tech.snnukf.dao.UserDao;
import tech.snnukf.service.UserService;
import tech.snnukf.spring.MyClassPathXmalApplicationContext;
import tech.snnukf.spring.MyFactory;

public class App 
{
    public static void main( String[] args )
    {
        //得到工厂实现
        MyFactory myFactory = new MyClassPathXmalApplicationContext("spring.xml");
        //得到对应的实例化对象
        UserDao userDao = (UserDao) myFactory.getBean("userDao");
        userDao.test();

        UserDao userDao02 = (UserDao) myFactory.getBean("userDao");
        userDao02.test();

        System.out.println(userDao == userDao02);

        UserService userService = (UserService) myFactory.getBean("userService");
        userService.test();
    }
}
```

## Spring IOC 核心技术

工厂模式（简单工厂 工厂方法 抽象工厂）

XML解析（Dom4j）

反射技术（实例化对象 反射获取方法 反射获取属性 反射获取构造器 反射调用方法）

策略模式（加载资源）

单例（IOC创建实例化对象）

## Spring IOC配置文件加载

### Spring 配置文件加载

spring.xml

### 根据相对路径加载资源

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml");
```

### 根据绝对路径加载资源

```java
ApplicationContext ac = new FileSystemXmlApplicationContext(path)
```

### 多配置文件加载

Spring框架启动时可以加载多个配置文件到项目环境中。

现在有service.xml dao.xml spring.xml等多个配置文件。

#### 可变参数，传入多个文件名

```java
ApplicationContext ac = new ClassPathXmlApplicationContext("spring.xml", "dao.xml", "service.xml");
```

#### 通过总的配置文件引入其他配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--导入需要包含的资源文件-->
    <import resource="service.xml">
    <import resource="dao.xml">
```

## Spring IOC 容器 Bean 对象实例化

### 构造器实例化 

通过默认构造器创建 空构造方法必须存在，否则创建失败。

```java
package tech.snnukf;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import tech.snnukf.dao.TypeDao;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
        BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring02.xml");
        TypeDao typeDao = (TypeDao) beanFactory.getBean("typeDao");
        typeDao.test();
    }
}

```



### 静态工厂实例化（了解）

注：

- 要有该工厂类及工厂方法
- 工厂方法为静态的

```java
//StaticFactory.java
package tech.snnukf.factory;

import tech.snnukf.service.TypeService;

/**
 * @author simple.jbx
 * @ClassName StaticFactory
 * @description 定义静态工厂
 * @email jb.xue@qq.com
 * @github https://github.com/simple-jbx
 * @date 2022/01/04/ 21:27
 */
public class StaticFactory {
    public static TypeService createTypeService() {
        return new TypeService();
    }
}

```

```java
        //静态工厂实例化
        TypeService typeService = (TypeService) beanFactory.getBean("typeService");
        typeService.test();
```

当我们指定Spring使用静态工厂方法来创建Bean实例时，Spring将先解析配置文件，并根据配置文件指定的信息，通过反射调用静态工厂类的静态工厂方法，并将该静态工厂方法的返回值作为Bean实例，在这个过程中，Spring不再负责创建Bean实例，而是由用户提供的静态工厂方法提供创建。

### 实例化工厂实例化（了解）

注：

- 工厂方法为非静态方法
- 需要配置工厂bean，并在业务bean中配置factory-bean、factory-method属性

1.定义工厂类

```java
package tech.snnukf.factory;

import tech.snnukf.controller.TypeController;
import tech.snnukf.service.UserService;

/**
 * @author simple.jbx
 * @ClassName InstanceFactory
 * @description 定义实例化工厂
 * @email jb.xue@qq.com
 * @github https://github.com/simple-jbx
 * @date 2022/01/04/ 22:32
 */
public class InstanceFactory {
   public TypeController createTypeController() {
       return new TypeController();
   }
}
```



2.设置配置文件

```xml
    <!--
    实例化工厂
        1.定义实例化工厂bean
        2.引用工厂bean 指定工厂创建方法（非静态）
        factory-bean 工厂对象对应的id
        factory-method 工厂类的方法
    -->
    <bean id="instanceFactory" class="tech.snnukf.factory.InstanceFactory"></bean>
    <bean id="typeController" factory-bean="instanceFactory" factory-method="createTypeController"></bean>
```

3.获取实例化对象

```java
        //实例化工厂实例化
        TypeController typeController = (TypeController) beanFactory.getBean("typeController");
        typeController.test();
```



### Spring三种实例化 Bean 方式比较

- 方式一：通过bean的缺省构造函数创建，当各个bean的业务逻辑相互比较独立的时候或者和外界关联较少的时候可以使用
- 方式二：利用静态factory方法创建，可以统一管理各个bean的创建，如各个bean在创建之前需要相同的初始化处理，则可以用这个factory方法进行统一的处理
- 方式三：利用实例化factory方法创建，即将factory方法也做了业务bean来控制，可以你敢于集成其他框架的bean创建管理方法，能够使bean和factory的角色互换。

## Spring IOC 注入