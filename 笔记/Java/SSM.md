# Spring Framework 系统架构

* Spring FrameWork 是Sping生态圈最基础的项目，是其他项目的根基

## 系统架构图

![](SSM.assets/image-20220626133718488.png)	

## 学习路线

![image-20220626133812395](SSM.assets/image-20220626133812395.png)

# Sping 核心概念的学习

* 代码书写现状

​	如果我们数据层需要修改，那么代码需要重新编译，发布，上线，比较麻烦。

​	![image-20220626134100532](SSM.assets/image-20220626134100532.png)



* 解决方案

​	使用对象的时候，不主动new提供对象，利用外部提供对象，下图只声明，不new对象。

![image-20220626134301406](SSM.assets/image-20220626134301406.png)

* 控制反转

  控制反转是对象的创建权转移到**外部**的一种思想。,平时我们用代码new来创建对象，如果把new的方式交给容器管理，那么这个就叫做控制反转。

* IOC容器 

  Spring提供了IoC容器，用来充当IoC思想中的外部，被管理的对象在Ioc容器中统称为**Bean**。可以绑定好Bean对象，让其不会缺少其他对象而无法运行。![image-20220626144034209](SSM.assets/image-20220626144034209.png)



* DI依赖注入

  在容器中建立bean与bean之间依赖关系过程，称为依赖注入。





# Ioc入门案例分析

* IoC管理的是：Service与Dao
* 用配置的方式告知IoC容器管理哪些对象
* 利用接口获取Ioc容器
* 利用接口方法获取Bean对象

1. 导入spring-context

   ```xml
       <dependencies>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
               <version>5.2.6.RELEASE</version>
           </dependency>
       </dependencies>
   ```

2. 定义Spring管理的类和接口

![image-20220626150940052](SSM.assets/image-20220626150940052.png)

3. 创建配置文件，指定bean

   ![image-20220626151004520](SSM.assets/image-20220626151004520.png)

4. 初始化IoC容器

   ```java
   ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
   context.getBean("Bean对象名")
   ```



# DI入门案例分析

1. 删除业务层中用new方式创建的dao对象，利用set方法，给这个接口提供对应的实现类

   ![image-20220626151848821](SSM.assets/image-20220626151848821.png)

2. 配置service与dao的联系

   1)name表示配置具体的属性

   2)ref表示给这个bookDao属性的set传一个Bean进去赋值。

   ```java
   <property name = "bookDao" ref = "bookDao"/>
   ```



# Bean配置基础

1. Bean可以加多个名称name，name可定义多个。

![image-20220626153326696](SSM.assets/image-20220626153326696.png)

2. Bean的单例模式(作用范围)

> Bean对象创建出来的可能是单例single(默认)或者多个prototype
>
> ```
> scope="singleton/prototype"
> ```

* 适合多个对象的bean：表现层、业务层、数据层、工具对象





# Spring实例化Bean

1. 构造函数

> Bean本质上就是创建对象，Spring会调用Bean对象的构造方法来创建,默认调用无参构造。（通过反射）

​	如果无参构造不存在，抛出异常==BeanCreationException==

2. 工厂实例化

​	1)什么是工厂模式 ?

   >工厂类会返回一个对象实例，调用者想创建一个对象，只要知道其名称就可以了，创建对象的时候不会对客户端暴露业务逻辑。

​	2)**静态**工厂实例化bean class="工厂类" factory-method="工厂实例方法" 这样就能得到工厂所返回的实例

```java
<!--方式二：使用静态工厂实例化bean-->
<bean id="orderDao" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>
```

​	3)动态工厂实例化bean

```java
<!-- 首先要创建实例工厂 后面再用factory-method 来创建工厂 -->
<bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
<bean id="userDao" factory-method="getUserDao" factory-bean="userFactory"/>
```

​	4)简化动态工程实例化bean

![	](SSM.assets/image-20220627165202602.png)

​	实现FactoryBean接口

```java
//FactoryBean创建对象 创建什么对象就用什么泛型
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }

    public Class<?> getObjectType() {
        return UserDao.class;
    }
}

applicationContext.xml
<!--方式四：使用FactoryBean实例化bean-->
    <bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>
```

# Bean的生命周期

Spring管理的bean对象是具有生命周期的，可以设置初始化和销毁函数

* 创建bean实例会触发init-method 

* java虚拟机关闭的时候，不会触发destroy-method，要用下面该两种方法。

  > 1. ClassPathXmlApplicationContext的实现类有close() 可以手动关闭触发析构函数
  > 2. registerShutdownHook

  ```java
  ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
  BookDao bookDao = (BookDao) ctx.getBean("bookDao");
  ctx.registerShutdownHook();//自动关闭容器的时候触发钩子
  ctx.close();//强行关闭容器 触发钩子
  ```

* 指定析构函数

  ```xml
  <bean id="bookDao" class="全类名" init-method="init" destroy-method="destory"/>
  ```

* 利用目标bean对象**实现接口**的形式，，控制bean的析构函数，可以避免向上面在xml对应接口

  ```java
  public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
      private BookDao bookDao;
  
      public void setBookDao(BookDao bookDao) {
          System.out.println("set .....");
          this.bookDao = bookDao;
      }
  
      public void save() {
          System.out.println("book service save ...");
          bookDao.save();
      }
  	//重写DisposableBean
      public void destroy() throws Exception {
          System.out.println("service destroy");
      }
  	//初始化InitializingBean 在属性设置完成以后 运行此操作
      public void afterPropertiesSet() throws Exception {
          System.out.println("service init");
      }
  }
  ```



# 依赖注入方式

## Sette注入

比如下面的book实现类,利用setter给bookDao赋值

```java
public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
    private BookDao bookDao;

    public void setBookDao(BookDao bookDao) {
        System.out.println("set .....");
        this.bookDao = bookDao;
    }
```

我们用配置的方式触发bean的set功能，在xml配置Bean对象内部添加 property

1) **ref**:引用bean对象

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"  scope="singleton"  init-method="init" destroy-method="destory"/>

<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
      <property name="bookDao" ref="bookDao"/>
 </bean>
```

2. value:传基础数据类型的值

```xml
    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <!--property标签：设置注入属性-->
        <!--name属性：设置注入的属性名，实际是set方法对应的名称-->
        <!--value属性：设置注入简单类型数据值-->
        <property name="connectionNum" value="100"/>
        <property name="databaseName" value="mysql"/>
    </bean>
```

