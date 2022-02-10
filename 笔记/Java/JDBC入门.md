## JDBC基本概念

JDBC是官方定义的接口，各大数据库厂商去实现接口，需要依赖jar包。



## 数据库连接操作

```
 //2. 加载进内存
Class.forName("com.mysql.cj.jdbc.Driver");

//3. 注册驱动 获取连接类
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db2", "root", "root");

//4. 自定义sql语句
String sql="SELECT * FROM emp";

//5. 用连接类 获取执行sql的对象
PreparedStatement prepareStatement  =connection.prepareStatement(sql);
ResultSet resultSet = prepareStatement.executeQuery();//执行语句 返回结果集

//6操作结果集
结果集的操作很多 这里忽略
```







## JDBC常用的几个类

### DriverManager驱动管理对象

功能有：

1. 注册驱动

   通过查看源码发现，DriverManager里面有**静态代码块**（加载类的时候调用，并且只会调用一次），实际上我们在注册驱动的操作时，是利用DriverManager来进行驱动注册的

   ```
   public class Driver extends NonRegisteringDriver implements java.sql.Driver {
       public Driver() throws SQLException {
       }
       static {
           try {
               DriverManager.registerDriver(new Driver());
           } catch (SQLException var1) {
               throw new RuntimeException("Can't register driver!");
           }
       }
   }
   
   ```

   

2. 获取数据库的连接

​	获取Connection对象，代表与数据源进行的唯一会话。

```java
 Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db2", "root", "root");//localhost 这一块是ip地址
```



### Connection:数据库连接对象

1. **获取执行sql 的对象**

   * CreateStatement
   * PreparedStatement

2. **管理事务**

   * 开启事务

     ```
     void setAutoCommit(boolean autoCommit) 调用false，即开启事务
     ```

   * 提交事务

     ```
     void conmit() //提交事务
     ```

   * 回滚事务

     ```
     rollback(); //回滚事务
     ```

### Statement：执行静态sql的对象

#### 执行sql

1. `int executeUpdate(String sql)`：执行DML 语句，**(insert,update,delete)**，DDL语句（create，alter，drop）数据库的设计比较复杂因此不常用。**返回值是影响的行数**
2. ResultSet executeQuery(String sql)：执行SELECT的sql，返回的是结果集

### Prepardment：执行预编译sql的对象



### ResultSet 结果集对象

* **next()** : 游标向下移动

* **getInt/String/Double**获取对应的数据类型

  参数说明：

  1. int :代表编号，从1开始
  2. String : 代表列名



### JDBC的异常处理

利用带参数的try...catch ，try结束以后，会自动释放在参数里面的资源

```
 try(//在变量里面声明驱动类
                Connection connection = DriverManager.getConnection("jdbc:mysql:///db1", "root", "root");
                PreparedStatement prepareStatement = connection.prepareStatement("insert demo values(?,'fxw')");
                ){
            //获取执行sql的对象
            prepareStatement.setString(1,""+1);
            int cnt=prepareStatement.executeUpdate();
            if(cnt>0){
                System.out.println("执行成功,影响"+cnt);
            }
            else{
                System.out.println("执行失败!");
            }
        }catch (SQLException throwables) {
            throwables.printStackTrace();
        }
```





## 抽取JDBC工具类 ：JDBCUtils

可以简化书写，避免冗余的重复代码，抽取方法和释放资源。

比如当我们释放JDBC的资源时，**普通**流程是这样的

1. 在try外面声明变量

2. **在finally释放资源前先判空，然后在释放资源，释放资源还要再rty...catch**，非常麻烦

因此我们可以将很多冗余的步骤封装到一个类里面实现我们的操作。

```配置文件
#配置文件
url=jdbc:mysql:///db1
user=root
password=root
driver=com.mysql.cj.jdbc.Driver
```



```

/**
 * JDBC工具类
 */
package jdbcUtils;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;
import java.util.Stack;

public class JDBCUtil {
    private static String user;
    private static String password;
    private static String url;
    private static String driver;
    /**
     * 初始化变量，从配置文件里读取信息到properties集合
     *
     */
    static{
        Properties properties=new Properties();

        try {
            // 加载文件进集合
            properties.load(new FileReader("D:\\mycode\\web\\src\\main\\java\\jdbcUtils\\jdbc.properties"));
            url=properties.getProperty("url");
            password=properties.getProperty("password");
            driver=properties.getProperty("driver");
            user=properties.getProperty("user");
            //注册驱动
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    /**
     * 连接数据库 获取Connection类
     * @return 链接类
     */

    public static Connection getConnection(){
        try {
            return DriverManager.getConnection(url,user,password);
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        return null;
    }

    /**
     * release resources
     * @param res 结果集
     * @param st 状态类（执行sql
     * @param cn 链接类（代表与数据源进行的唯一会话
     */
    public static void close(ResultSet res, Statement st,Connection cn){
        if(res!=null){
            try {
                res.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(st!=null){
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(cn!=null){
            try {
                cn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }

    /**
     * release resources
     * @param st
     * @param cn
     */
    public static void close(Statement st,Connection cn){
        if(st!=null){
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(cn!=null){
            try {
                cn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }

}
```





## JDBC事务管理

1. 事务：多个步骤，要么同时成功，要么同时失败
2. 流程操作：
   * 开启事务
   * 提交事务
   * 回滚事务

3. 使用connection对象来管理事务操作

   * 开启事务

   ```
   void setAutoCommit(boolean autoCommit) 调用false，即开启事务
   ```

   * 提交事务

     ```
     void conmit() //提交事务
     ```

   * 回滚事务

     ```
     rollback(); //回滚事务
     ```



```
package jdbcTest;

import jdbcUtils.JDBCUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

/**
 * jdbc管理事务
 */
public class JdbcCommit {
    public static void main(String[] args) {
        Connection connection =null;
        PreparedStatement preparedStatement=null;
        PreparedStatement preparedStatement2=null;
        try{
            connection=JDBCUtil.getConnection();
            connection.setAutoCommit(false);//关闭自动提交 开启事务
            String sql="UPDATE stu SET age=? where name=?";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement2 = connection.prepareStatement(sql);

            preparedStatement.setInt(1,0);
            preparedStatement.setString(2,"张三");

            preparedStatement2.setInt(1,0);
            preparedStatement2.setString(2,"张四");

            preparedStatement.executeUpdate();
            int a=3/0;//手动制造错误
            preparedStatement2.executeUpdate();

            //到这说明执行成功了 就提交
            connection.commit();
        }
        catch (Exception e){
            //出现了异常 回滚 
            //连接对象可能有null的时候 (连接不到) 所以用可能为Null的对象的时候 好习惯是先判断是否为空
            try {
                if(connection!=null){
                    connection.rollback();
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }

            e.printStackTrace();
        }
        finally {
            JDBCUtil.close(preparedStatement,connection);
            JDBCUtil.close(preparedStatement,null);
        }

    }
}

```



##  数据库连接池

用户不断地申请资源和释放，对资源浪费较大，效率较差。

因此在一个大**池子**放连接对象，每次用户调用地时候，就从里面拿。

**数据库连接池：**

一个存放数据库连接的容器(集合)，当系统初始化好后，容器被创建，容器中会申请一些连接，用户用的时候取出来，用完放回去。

**数据库连接池优点：**

1. **复用连接，提高连接利用率**（**节约资源**）
2. **反复连接**要对系统底层进行请求，速度比较慢。（**高效**）





### 数据库连接池的实现

1. 标准接口：DataSource  javax.sql
   1. 方法：
      * 获取连接：getConnection
      * 归还连接：连接对象是从Connection获取的，我们close的时候，不会关闭连接，而是变成归还连接。
2. 一般不去实现，有数据库厂商来实现
   1. C3P0：数据库连接池技术
   2. Druid：数据库连接池技术，由阿里巴巴提供



## C3P0使用步骤 

1. 导入jar
2. 定义配置文件（也可以用代码配置，但是不推荐）
   * 名称：c3p0.properties 或者 c3p0-config.xml 
   * 路径：直接放在src目录下即可，会自动搜索
3. 创建数据库连接池对象

```
DataSource dataSource=new ComboPooledDateSource();//c3p0的实现类

Connection con =dataSource.getConnection();//获取连接对象 即可使用
```



## Druid

1. 导入jar包

2. 定义配置文件

3. 加载配置文件 Properties 

   可以用字符流也能用字节流，下面代码是，获取本类的类加载器，查找资源文件并转化成字节流返回。

4. 获取连接池对象DataSource

5. 获取连接对象

```java
public class DruidTest {
    public static void main(String[] args) throws Exception{
        //类加载器下 根据指定名字寻找resources下的对应资源 并且返回字节输入流
        InputStream inputStream=DruidTest.class.getClassLoader().getResourceAsStream("druid.properties");
   
        Properties properties=new Properties();
        properties.load(inputStream);
        
        DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
        Connection connection= dataSource.getConnection();
    }
}
```



### druid.properties资源文件

配置数据源的参数...

```
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql:///db1
username=root
password=root
initialSize=5
maxActive=10

# 最大等待时间，超过时间将接到异常
maxWait=3000
```



### Druid连接池工具类

1. 定义JDBCUtils
2. 静态代码块加载配置文件，初始化连接池对象



1. 工具类提供的方法：
   1. 获取连接方法：通过数据库连接池连接
   2. 释放资源
   3. 获取连接池方法

```
/**
 * 连接池的工具类
 */
public class JDBCUtils {
    private static DataSource dataSource;
    static {
        Properties properties=new Properties();
        try {
            //用类加载器获取字符流 加载资源文件
          properties.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            dataSource= DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取连接
     * @return 连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }

    /**
     * 释放资源(归还连接)
     * @param ps
     * @param con
     * @param res
     */
    public  static  void close(PreparedStatement ps, Connection con, ResultSet res){
        if(ps!=null){
            try {
                ps.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(con!=null){
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

        if(res!=null){
            try {
                res.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }

    }

    /**
     * 释放资源
     * @param ps
     * @param con
     */
    public  static  void close(PreparedStatement ps, Connection con){
        if(ps!=null){
            try {
                ps.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(con!=null){
            try {
                con.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }

    public static DataSource getDataSource(){
        return dataSource;
    }
}
```

## Spring JDBCTemplate

* Spring框架对JDBC的简单封装



###  JDBCTemplate构造

**参数传**的是数据源DataSource,数据源的实现类用的是**DruidUtils**的封装，如果在构造的时候没传，后面也可以调用方法手动传数据源

```
JdbcTemplate jdbcTemplate=new JdbcTemplate(JDBCUtils.getDataSource());//JDBCUtils 获取数据源
```



### JDBCTemplate实现增删改操作（DML）

* queryForMap：

```
String sql="insert into user values(?,?)";
jdbcTemplate.update(sql,"admin","123");//执行 sql + 参数列表 会自动替换参数 和prepareStatement机理类似
```

### JDBCTemplate实现查询操作（DQL）（重点）

1. **queryForMap ** ：将结果封装成一个map，列名为键，值为值 **只支持一条记录**

```
        //查询
        sql="select * from user where username=? AND password=?";
        Map<String, Object> res = jdbcTemplate.queryForMap(sql, "admin", "123456"); //
        for(Map.Entry<String,Object>entry:res.entrySet()){//遍历 也能直接输出
            System.out.println(entry);
        }
```



2. **queryForList**：查询所有的记录，获取每条记录的Map,存到List里面，遍历方式也很简单foreach

```
        sql="select * from user";
        final List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        for(Map<String,Object>mp:maps){
            System.out.println(mp);
        }
```



3. **query**：返回的是List 需要实现RowMapper

* **自己实现RowMapper 接口**，对每一个结果集操作封装成User存到List 里面去，也可以用**Bean实现类**

```
        //query 返回指定封装对象的list集
        sql="select * from user";
        List<User> list = jdbcTemplate.query(sql, new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet resultSet, int i) throws SQLException {
                String username=resultSet.getString(1);
                String password=resultSet.getString(2);
                return new User(username,password);
            }
        });
        for(User i:list){
            System.out.println(i);
        }
```



* **BeanPropertyRowMapper**是**RowMapper**的实现类，帮我们做**封装结果集到对象**这一步操作，确保有无参构造，**BeanPropertyRowMappe**的构造参数为对象的类。

* RowMapper是

  ```
  @Test
  public void test(){
      JdbcTemplate jdbcTemplate = new JdbcTemplate(JDBCUtils.getDataSource());
      String sql="select * from user";
      List<User> list2 = jdbcTemplate.query(sql, new BeanPropertyRowMapper<User>(User.class));
      for (User i : list2) {
          System.out.println(i);
      }
  }
  ```

值得一提的是，对于User的每一个成员函数都会封装成类，因此double，int，需要用其对应的封装类。



4. queryForObject：一般查询集合函数，将结果封装成对象。

   ![image-20210609005245901](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210609005245901.png)

## JDBCTemplate方法总结

| 函数           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| update         | 增删改 DML操作                                               |
| queryForMap    | DQL，查询一条语句，封装成map                                 |
| queryForList   | 同上，查询多条语句，封装成map后存储到list                    |
| query          | 封装规则需要自己指定，也可以用Bean实现类简化操作，需要保证字段名相互匹配 |
| queryForObject | 一般查询聚合函数                                             |



## 总结

| 连接方式             | 使用方式                                                     |                    |
| -------------------- | ------------------------------------------------------------ | ------------------ |
| 普通连接             | 首先将Driver加载进内存，Driver的静态代码块里DriverManager进行注册，再用DriverManager获取连接对象Connection，连接对象获取statement执行对象，执行sql，需要不断地申请、释放资源。 | 实现麻烦           |
| 连接池连接           | 首先准备好配置文件，用Druid工厂类获取数据源，然后获取连接对象，也需要不断取、归还资源，代码麻烦 | 效率变高，实现麻烦 |
| JDBCTemplate框架连接 | 直接执行sql，很方便。                                        | 封装性强           |
