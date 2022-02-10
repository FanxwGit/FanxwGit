## JSP技术

* 全称：java Server packges：java服务端页面，可以写java代码和html页面



### JSP原理

服务器请求资源的时候会将.jsp文件转化为.java文件，其本质上就是servlet。



### JSP脚本

1. <% %>：定义的java代码，在service方法中
2. <%! %>：定义的是全局成员变量，在jsp转化后的java类的成员位置中，比较少，容易出现线程问题
3. <%=