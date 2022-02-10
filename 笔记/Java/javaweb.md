# Tomocat

## 项目部署

1. 直接将项目放到webapps目录下即可
   1. 项目访问路径-->虚拟路径
2. 将项**目打包成war**包，将war包放置到webapps目录下

## java目录结构

java项目

​	项目名

​		--WEB-INF

​			--web.xml 该项目的核心配置文件

​			--classes目录，放字节码文件

​			--lib目录，放项目依赖的jar包

# Servlet

## 生命周期

### init()

> 创建于用户第一次调用对应URL 时，也**可以指定** Servlet 在服务器第一次启动时被加载
>
> 调用一个 Servlet，创建一个实例，每一个用户请求都会产生**一个新的线程**，适当的时候移交给 doGet 或 doPost 方法。init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。

* 注意： 每一个用户请求都会产生**一个新的线程**，所以可能会出现进程安全问题，因此对于Servlet，我们一般不设置成员属性，即便设置了也不会用函数去改变其数值。

### service()

是执行实际任务的主要方法，调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端，每次收到service请求，都会产生一个新的线程并且调用服务，检查HTTP的请求类型，并且在适当的时候调用**doGet**、**doPost**、doPut，doDelete 等方法。

### destroy() 

一般用于:

1. Servlet 关闭数据库连接
2. 停止后台线程
3. 把 Cookie 列表或点击计数器写入到磁盘
4. 执行其他类似的清理活动

执行完以后，servlet对象被标记为垃圾回收，被JVM回收





## 注解配置

Servlet3.0后提供了**注解配置**

```
@WebServlet(name="类名", urlPatterns="地址")
```

类名可以不写，就留urlPatterns即可

```
@WebServlet("地址")
```

**注意：**当多个客户端需要使用此Servlet时，地址可以填写多个，如

```
@WebServlet({"/123","/456","789"})
```

路径可以使用通配符，如：

```
@WebServlet("/demo/demo01")
可以写成
@WebServlet("/demo/*")
```

可以给文件名加自定义扩展名

```
@WebServlet("*.do");//do可以是任意的字符
```



## IDEA和tomcat的相关配置

1. idea里面设置端口号之类的.... 实际在tomcat里面的conf里面进行配置更改
2. idea里面写的代码**载入服务器**，会进入webapps下面。并且**WEB-INF**是Java的WEB应用的**安全目录**。**客户端无法访问**，只有服务端可以访问的目录
3. 小虫子调试可以进行debug的调试





# HTTPServlet

## HTTP

* 概念：Hyper Text TransFer Protocol 超文本传输协议
* 传输协议：发送的数据格式
* 特点：

  * 基于TCP/IP的高级协议
* **默认端口**:80
  * 基于请求相应模型：相应请求一一对应
* 无状态协议,**每次请求独立**，无法交互数据。
  * 历史版本特点

    * 1.0:每次相应都会建立连接
  * 1.1:复用版本

### 请求消息

#### 请求消息的结构

* 请求行：请求方式，请求url，请求协议/版本
* 请求头：客户端告诉浏览器一些信息。
* 请求空行：空行，分割请求头和请求体。
* 请求体(正文)：封装POST请求消息的请求参数

![image-20210417110649437](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210417110649437.png)

#### 请求行

```
POST/GET /URL HTTP/1.1
```

#### 请求头，请求头值

```
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:86.0) Gecko/20100101 Firefox/86.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 0
Origin: http://localhost:8080
Connection: keep-alive
Referer: http://localhost:8080/javaWeb_war/
Cookie: Phpstorm-bfdf9589=586bc0e8-88ca-4a4e-9a2c-82d7f2df0982
Upgrade-Insecure-Requests: 1
```

User-Agent：告诉服务器现在的浏览器信息——可以解决浏览器兼容性的问题。

Accept：告诉服务器，这个浏览器能够接收怎样的格式。

Accept-Language:可以接受的语言

Accept-Encoding:可以接受的压缩格式

**Referer**:告诉服务器**请求哪里来**，可以用来防外。

Connection:连接状态，是否能被复用



#### 请求体(正文)



### 请求方式(get/post)

请求方式很多，常用的就这两种。

GET：

	1. 请求参数在请求行，url之后。
	2. 请求url长度有限制
	3. 不太安全

POST:

1. 请求参数在**请求体**中
2. 请求的url长度没有限制
3. 相对安全





### 响应消息

响应头：

   1. 格式：头名称： 值

        2. 常见的响应头：

             1. **Content-Type**：服务器告诉客户端本次响应体数据格式以及编码格式

                 	2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据

                  * 值：
                      * in-line:默认值,在当前页面内打开
                         * attachment;filename=xxx：以附件形式打开响应体。文件下载

           2. 响应空行

           3. 响应体:传输的数据

           响应字符串格式
           	HTTP/1.1 200 OK
           	Content-Type: text/html;charset=UTF-8
           	Content-Length: 101
           	Date: Wed, 06 Jun 2018 07:08:42 GMT

	<html>
	  <head>
	    <title>$Title$</title>
	  </head>
	  <body>
	  hello , response
	  </body>
	</html>



## 请求相应原理![image-20210417111755554](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210417111755554.png)
1. 客户端发送请求，服务器接收，并且根据对应的url，搜索并创建对应的servlet。
2. 创建requeset,response服务器封装信息到request之中
3. 服务器将两对象传参给service,doget,dopost，之类的方法。
4. 服务器根据我们设置的response做出相应返回给客户端。

## Request
### request，response对象的原理

1. 都是服务器创建，我们来用
2. request对象获取请求消息，response设置相应消息

### HttpServletRequst对象继承体系
ServletRequst -- **接口**
	| |   继承
HttpServletRequst -- **接口**
	| |   服务器实现
org.apache.catalina.connector.RequestFacade类（tomcat)

### resquest功能

#### 获取请求消息数据

1. 获取请求行数据

   ```
   GET/ javaweb/demo01?name=123 HTTP/1.1
   ```

   * 获取请求方式

     `getMethod()`

   * **获取虚拟目录**:javaweb

     `getContextPath()`

   * 获取Setvlet路径:demo01

     `getServletPath()`

   * 获取get方式的请求参数：

     `getQueryString()`

   * 获取**URI**/URL(更长了): javaweb/demo01 /  http://localhost/javaweb/demo01

     `getRequesURI() `

     `getRequesURL()`

     **个人理解的URL 和 URI的区别**

     URL唯一定位，如：中国/福建省/厦门市/张三/身份证号123

     URI唯一标识，如：/身份证123

     因为每个定位都可以代表是一个标识，因此URL是URI的子集

     

   * 获取协议以及版本：HTTP/1.1

     `getProtocol()`

   * 获取客户端的ip地址：

     `getRemoteAddr()`

     
   
2. 获取请求头数据

   ```
   getHeader("请求头名称");//不区分大小写
   String agent=req.getHeader("user-agent");//获取浏览器信息
   String Referer=req.getHeader("referer");//获取来源，直接访问为null
   ```

3. 获取请求体数据

...

#### 获取请求参数的通用方式

(不论是get还是post都可以用下列方法来获取请求参数)

```
String getParameter(String name);//根据参数名获取参数值 常用
String []getParameterValues(String name);//根据参数名获取参数数组  列如多个参数 有多个值 name="123"&name="666" 
```

#### 请求转发

1. 获取getRequestDispatcher转发器对象，调用forward方法

```
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/CountServlet");
        requestDispatcher.forward(req,resp);//传递参数
```



* **特点：**
  * 地址栏不变
  * 只能访问当前项目的资源路径
  * 转发是**一次请求**



那么如何在转发页面之间传递数据呢？ 可以把数据存储到**reques**的数据域(Attribute)里面，把数据放到请求体里面传给下一个网页。在整个请求链中共享数据，可通过getAttribute、setAttribute将数据带到jsp页面中进行显示。

```
req.setAttribute("msg","hello");//传递一个对象  req.setAttribute(键值,对象)
req.getRequestDispatcher("/CountServlet").forward(req,resp);

// 另外一个页面
String name= (String) req.getAttribute("msg");//根据键 获取值   并且需要转型
```





# cookie

## 操作Cookie步骤

1. 创建Cookie对象
2. 发送Cookie
3. 获取Cookie

```
//设置cookie
Cookie c=new Cookie("msg","hello");
response.addCookie(c);
//获取cookies
Cookie [] cs=request.getCookies();
for(Cookie cc:cs){
	System.out.println(cc.getName()+"="+cc.getValue());
}	
```



## Cookie的存活时间

默认情况下，浏览器关闭，Cookie数据被销毁

* 持久化存储
  * setMaxAge(int expiry)设置时间
    * 1.  expiry>0为存活的秒
      2.  expiry=0为清除cookie
      3.  expiry<0为暂时缓存在浏览器上，关闭就清除





## cookie共享问题



1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie默认情况下cookie不能共享

     * setPath(String path):设置cookie的获取范围。默认情况下，是当前的虚拟目录

       * 如果要共享，则可以将path设置为"/"

       



# JSP技术

## 概念：

* Java Server Pages： java服务器端页面
	* 可以理解为：一个特殊的页面，可以写html标签，又可以写java代码
	* 用于简化书写！！！

## 原理

* **JSP本质上就是一个Servlet**



## 指令

* 作用：用于配置JSP页面，导入资源文件

* 格式：`<%@ 指令名称 属性名1=属性值1 属性名2=属性值2>...`

* 指令分类

  1. page：配置jsp页面

     * contentType 响应头

     ```
     <%@ page contentType="text/html;charset=gbk">
     ```

     * import 导包

     ```
     <%@ page import="java.util.List" %>
     ```

     * error 错误信息

     在我们页面出现异常的时候，避免用户看到异常页面

     ```
     errorPage="500.jsp" 
     ```

     * isErrorPage 标识是否是错误页面

     标识后，就能用**内置对象exception**，方便以后打印到日志里。

     ```
     isErrorPage="true";
     <% String msg =exception.getMessage();%>
     ```

     

  2. include：页面包含，导入页面的资源文件（比如某些页面，可以封装到jsp里面，不用重复去写了）

     ```
     <%@ include file="top.jps"%>
     ```

     

  3. taglib：导入资源

     一般用来导入标签库。

## 脚本

JSP的脚本：JSP定义Java代码的方式

1. <%  代码 %>：定义的java代码，在**service方法中**。service方法中可以定义什么，该脚本中就可以定义什么。
2. <%! 代码 %>：定义的java代码，在jsp转换后的java类的成员位置。
3. <%= 代码 %>：定义的java代码，会输出到页面上。输出语句中可以定义什么，该脚本中就可以定义什么。



## 内置对象

由于本质是Servlet，所以有内置的成员对象。在jsp页面中不需要获取和创建，可以直接使用的对象

jsp一共有9个内置对象，4个域对象（**域对象用来共享数据**），5个

9个变量名为：

* pageContext（PageContext）：
* requeset(HttpServletRequest) : 
* session(HttpSession) 
* application(ServletContext)



* response(HttpServletResponse)

* page(Object page =this)

* out(JSPWriter)：

  * 字符输出流对象。可以将数据输出到页面上。和response.getWriter()类似

     * response.getWriter()和out.write()的区别：
       * 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据。

      *  response.getWriter()数据输出**永远在out.write()之前**

* config(Servlet config)

* exception(Throwable)：只有在isErrorPage="true"的时候才能用

| 变量名      | 数据类型            | 作用                                            |
| ----------- | ------------------- | ----------------------------------------------- |
| pageContext | PageContext         | **当前页面**共享，还可以获取其他8个**内置对象** |
| requeset    | HttpServletRequest  | 一次请求访问的多个资源（转发）                  |
| session     | HttpSession         | 一次会话的多个请求                              |
| application | ServletContext      | 所有用户间共享数据，唯一的一个对象。            |
| response    | HttpServletResponse | 响应对象                                        |
| page        | Object page =this   | 当前页面(Servlet)的对象                         |
| out         | JSPWriter           | 输出对象，可以把页面输出到页面                  |
| config      | Servlet config      | Sevlet的配置对象                                |
| exception   | Throwable           | 异常对象                                        |

# Session

## 操作步骤

* 获取Seesion getAttribute()
* 设置Session setAttribute()
* 删除Session removeAttribute()



## 原理

1. 首次浏览器访问服务器，未携带SessionId,服务器创建Session,并且将Sessionid 用cookie传给客户端
2. 客户端下次访问，携带SessionId，服务器就会找到对应的Session，不会出错。



## 细节

1. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

不是同一个，但是要确保数据不丢失。tomcat**自动完成**以下工作
   * session的钝化： 在服务器正常关闭之前，将session对象系列化到硬盘上
   * session的活化：在服务器启动后，将session文件转化为内存中的session对象即可。

注意：IDEA由于会自动删掉work文件夹,因此无法读取。但是直接Tomcat部署文件可以。



2. 当客户端关闭后，服务器不关闭，两次获取session是否为同一个？

通常来说，不是同一个。

因为客户端关闭了，携带的SessionId也随之销毁，但是可以通过修改存活时间来实现，重启客户端，也能自动获取同一个Session。

```
```



3. session什么时候被销毁？
		1. 服务器关闭
		2. session对象调用invalidate() 。
		3. session默认失效时间 30分钟
			选择性配置修改	
	```
			<session-config>
		        <session-timeout>30</session-timeout>
		    </session-config>
	```



## session的特点


1. session用于存储**一次会话的多次请求**的数据，存在服务器端
 2. session可以存储任意类型，任意大小的数据

* session与Cookie的区别：
	1. session存储数据在服务器端，Cookie在客户端
	2. session没有数据大小限制，Cookie有
	3. session数据安全，Cookie相对于不安全



# MVC开发模式

 

## sp演变历史

早期只有Servlet，很麻烦，后面有了jsp，简化了Servlet的开发，但是jsp写大量的java代码，又写html标签，造成难于维护，难于分工协作。再后来，java的web的开发就借鉴了mvc开发模式，使得程序的设计更加的合理。



## MVC介绍

1. M：model 模型 JavaBean

   完成业务操作，如：查询数据库，封装对象

2. V：view 视图 JSP（jsp一般就不写java代码了，耦合性太强.... 用EL表达式替换

   展示数据

3. C Controller 控制器 Servlet

   获取用户输入

   调用模型

   将数据交给视图展示

## 优点

耦合性低，方便维护，可以利于分工协作





# EL表达式

* 概念：表达式语言

* 作用：替换简化jsp页面中的java代码的编写

* 语法： ${ .....}

* 注意：jsp默认支持el表达式，也可以在page配置的时候，让代码忽略el表达式，也可以忽略单个的表达式

  * 方法一：page设置 isELignored="true";
  * 方法二：在对应代码面前加反斜杠\

  

## EL运算符

1. 算术运算符：+ - * / 

2. 比较运算符 > < >= <= == !=

3. 逻辑运算 && ||  ！(not)

4. **空运算符** empty

   用于判断字符串，集合，数组是否为空 或 长度为0  ： \${empty list}   \${not empty list}



## EL获取域中值

el表达式只能从4大**域对象里获取值**。

* 语法：
  1. ${域名.键名} ：从指定域里获取指定的键值 ： \${requestScope.name} 
     * 域名：
       1. pageScope	   -> pageContext
       2. requsetScope  -> request
       3. sessionScope   -> session
       4. applicationScope -> application (ServletContext)
  2. ${键名}：表示从**最小的域开始依次查找**（按上述顺序，pageScope开始），找到值为止。

值得注意的是：**当获取的键值为空的时候，就会显示空白，如果想要用request还需要用pageContext获取内置对象**

比如虚拟路径：${pageContext.request.contextPath}

## EL获取对象值



### 类对象

```

public class MyUser {
    private  String name;
    private  String age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }

}
```

```
<%@ page import="java.net.http.HttpRequest" %>
<%@ page import="user.MyUser" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<%
  MyUser myUser =new MyUser();
  myUser.setName("张三");
  myUser.setAge("14");
  request.setAttribute("u", myUser);
%>
<html>
  <head>
    <title>el获取值</title>
  </head>
  <body>
  <h1>获取值</h1>
  ${u.name}//根据命名规则，会自动调用getName方法
  </body>
</html>
```

当我们 u.name的时候，首先从每个域找键为u的键值对，找到了以后调用其**getName**方法，如果没找到对应的get方法，就会报500的错误。

所以我们可以写 getAbc,在jsp页面写el表达式的时候，可以写${u.abc}，比如在日期，我们就能手写这个函数获取格式化的对象。





### List集合

用索引访问，当索引越界了会自动为空。

${list[0]}

### Map集合

1. 用索引键访问 ： map["name"]
2. 用.访问：map.name




## EL获取隐式对象

1. pageContext：获取JSP的其他8个内置对象

如：

```
<h1>动态获取虚拟目录</h1>
${pageContext.request.contextPath}
```

# JSTL

javaServer Pages Tag Library JSP 标准标签库

它主要提供给Java Web开发人员一个标准通用的标签库，开发人员可以利用这些标签取代JSP页面上的[Java](https://baike.baidu.com/item/Java/85979)代码，从而提高程序的可读性，降低程序的维护难度。

* 作用：因为java代码尽量不写，所以JSTL用于简化和替换jsp页面上的java代码

## 使用步骤

1. 导入jar，用tablig 引入标签库，一般人前缀都叫c

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
```

## 常用的JSTL标签

### if标签

test="" jie'shou

```
  <c:if test="true">你好</c:if>
  <c:if test="false">不好</c:if>
```

### choose标签

相当于swich语句，可以替代if else

```
  <c:choose >
    <c:when test="${list==null}">你好6</c:when>
    <c:otherwise>你不6</c:otherwise>
  </c:choose>
```

### foreach标签



![image-20210613165211694](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210613165211694.png)

* 普通遍历

```

<c:forEach var="i" begin="0" end="10" step="1" varStatus="item">
  <p>${item.index} ${2*i}</p>
</c:forEach>

//容器
<c:forEach var="i" begin="0" end="${list.size()-1}" step="1" varStatus="item">
  <p>${requestScope.list[i]} ${item.index}</p>
</c:forEach>
```

* 增强for容器遍历 只需要增加items="${容器}"

```
<c:forEach items="${list}" var="i" varStatus="vs">
    <p>${vs.index} ${i}</p>
  </c:forEach>
```

# 三层架构

1. 界面层 看得到的界面
2. 业务逻辑层 处理业务逻辑 **组合**dao层的方法
3. 数据访问层 操作数据存储文件的 

![image-20210613173848386](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210613173848386.png)



# Filter过滤器

当访问服务器的时候，过滤器可以将请求拦截下来，完成一些特殊的功能



## 入门操作

定义一个类，实现接口Filter

1. 定义类

2. 复写方法

3. 配置(个人测试，vlues 和urlPatterns 没很大的区别)

   * 路径配置 urlPatterns  (可以注解配置，也能xml配置)

     ```
     @WebFilter(urlPatterns = {"index.jsp","test1.html"})
     public class Filter01 implements Filter {
     }
     ```

   * 拦截方式配置：资源被访问的方式

     * 注解配置：设置dispatcherTypes属性

     1. REQUEST：默认值：浏览器直接访问资源
     2. FORWARD：转发访问资源
     3. INCLUDE：包含访问资源
     4. ERROR：错误跳转资源
     5. ASYNC：异步访问资源

4. 过滤器执行流程

   执行过滤器->执行放行后的资源->回来执行过滤器放行代码后的内容

5. 过滤器的生命周期

   1. init：初始化，服务器启动后执行
   2. doFilter：每次被拦截时执行
   3. destroy：服务器**正常**关闭以后执行。

6. 过滤器链（配置多个过滤器）
   * 执行顺序：如果是注解，按照字典序排序，值小的先执行，如果是web.xml配置：\<filter-mapping>谁定义在上面，谁先执行



## 增强对象的功能——代理模式

* 设计模式：一些通用的解决事情方式的流程

  * 装饰模式:

  * 代理模式： **proxy**

    * 真实对象:被代理的对象
    * 代理对象
    * 代理模式：代理对象代理真实对象，达到增强功能的目的

    实现方式有

    1. 静态代理，有一个类文件描述的代理方式
    2. 动态代理，在内存中形成代理类

    

## 动态代理实例分析

* 一句话总结：就是代理你原本的类，并且修改其方法（参数，逻辑值，返回值）。

1. 创建接口

```
public interface cat {
    int c(int a);
}
```



2. 编写实现类

```
public class catimp implements cat{

    @Override
    public int c(int a) {
        System.out.println("方法被调用了");
        return a;
    }
}
```



3. 创建动态代理

```
cat q=new catimp();//真实对象

//创建代理实现类，并且强制转换
cat proxy_cat =(cat) Proxy.newProxyInstance(q.getClass().getClassLoader(), q.getClass().getInterfaces(), new InvocationHandler() {

//每次执行都会调用invoke函数
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method.getName());
                if(method.getName().equals("c")){
                    int x=(int)args[0]+1;
                    return method.invoke(q,x);
                }
                else{
                    return method.invoke(q,args);
                }
            }
        // method.invoke是调用方法执行
        //invoke的返回值是方法的返回值，如果你的函数没有返回值 可以直接返回null
        //如果用原来的返回值 就return method.invoke
    }
});
proxy_cat.c(3); //迪奥用方法
```

* **创建动态代理**

1. 调用Proxy.newProxyInstance
2. 实现InvocationHandler接口作为参数

* **InvocationHandler三个参数**

1. proxy 代理对象（不常使用）

2. method 代理对象调用的方法被封装为的对象 (Method)
3. args 方法执行时，传递的实际参数

* **invoke函数注意事项**

1. 每次函数调用的时候都会触发
2. 返回值就是函数的返回值，如果函数没有返回值可以写null

* **代理实现类的作用：**

1. 增强参数
2. 增强逻辑
3. 增强返回值

## 代理实现类的实战分析——替换敏感词

* 业务需求：如果字符串里包含了敏感词汇，就进行替换。

* 设计思路：在拦截器初始化的时候调用IO，在执行拦截的时候，创建代理类，最后放行时，返回代理类即可。

1. 将敏感词汇txt存入list之中。

![image-20210714193447898](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210714193447898.png)

2. 创建拦截器，并且将文件信息读入铭感词汇之中

```
private List<String> list;
 //初始化的时候加载资源
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // 1. 在web下的路径，我们根据filterConfig获取ServletContext,调用其方法获取真实路径,格式为 /WEB-INF/classes/文件名
        String realPath = filterConfig.getServletContext().getRealPath("/WEB-INF/classes/word.txt");
        System.out.println(realPath);
        // 2. 获取字符输入流,注意抛出异常
        FileReader fileReader=null;
        try{
            fileReader=new FileReader(realPath);
            // 3.从字符输入流读取文本，缓冲字符，以提供字符，数组和行的高效读取——BufferedReader
            BufferedReader bufferedReader = new BufferedReader(fileReader);
            // 4.存入List
            String line=null;
            list=new ArrayList<String>();
            while((line=bufferedReader.readLine())!=null){
                list.add(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```

3. 创建req的动态代理，放行时将动态代理作为参数传入

```
@Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        ServletRequest  proxy_req= (ServletRequest)Proxy.newProxyInstance(servletRequest.getClass().getClassLoader(), servletRequest.getClass().getInterfaces(), new InvocationHandler() {
            //每次执行函数的时候都会执行这个 最后要返回  真实对象的方法的执行对象
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if(method.getName().equals("getParameter")){
                    //先调用一下，获取原来的值
                    String res = (String) method.invoke(servletRequest,args);
                    for(String v:list){
                        if(res.contains(v)){
                            res=res.replaceAll(v,"***");
                        }
                    }
                    return res;//返回真实值
                }
                else{
                    return method.invoke(servletRequest,args);
                }
            }
        });
        filterChain.doFilter(proxy_req,servletResponse);//放行，放入(代理的)
    }
```

4. 表单输入

```
<form method="post" action="Servlettest01">
    <input name="text" value="123">
    <input type="submit">
</form>
</body>
</html>
```

# Listener:监听器

* 概念：web三大组件之一
* 事件：一件事
* 事件源：事件发生的地方
* 监听器：负责监听的对象
* 注册监听：事件，事件源，监听器绑在一起，当事件源发生后，执行监听器的代码

* 步骤：
  1. 定义类，实现servletContextListener接口
  2. 复写方法
  3. 配置（xml，注解）
     1. 注解：直接+WebLinster

* **作用**：ServletContext生成的时候可以加载资源进内存，可以根据配置在xml的信息，getInitParameter来调用初始化在web.xml中存放的参量，获取资源真实地址，从而加载进内存

```
import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

@WebListener
public class linstener implements ServletContextListener {
    //创建服务器后调用
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        //常用来配置文件

        ServletContext servletContext = sce.getServletContext();

        // 2.根据配置xml，获取配置文件的名  用getInitParameter来调用初始化在web.xml中存放的参量
        String name = servletContext.getInitParameter("name");

        System.out.println(name);
    }

    //创建服务器前调用
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        //常用来释放资源
    }
}
```



![image-20210715175411918](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210715175411918.png)


