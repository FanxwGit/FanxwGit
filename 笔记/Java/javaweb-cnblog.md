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

![image-20210417110649437](https://img2020.cnblogs.com/blog/2212500/202106/2212500-20210610131111051-1015684024.png)

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

## 请求相应原理![image-20210417111755554](https://img2020.cnblogs.com/blog/2212500/202106/2212500-20210610131110515-2094135962.png)
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

#### 获取请求参数的通用方式(不论是get还是post都可以用下列方法来获取请求参数)

```
String getParameter(String name);//根据参数名获取参数值 常用
String []getParameterValues(String name);//根据参数名获取参数数组  列如多个参数 有多个值 name="123"&name="666" 
```

#### Tomocat中文乱码问题

 **get**:tomcat8之后解决乱**码**

**post**:会乱码，获取获取参数前，设置request编码，`request.setCharacterEncoding("UTF-8")`



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



># ServletRequest对象域
>
>## 1.生命周期
>
>在service方法调用前由服务器创建，传入service()方法，整个请求结束，ServletRequest生命周期结束。
>
>## 2.作用范围
>
>整个请求链。（请求转发也存在，请求转发属于一次请求。）
>
>## 3.作用
>
>- 在整个请求链中共享数据，主要可通过getAttribute、setAttribute，（移除removeAttribute）将数据带到jsp页面中进行显示。





​	