# Ajax

* 概念： 异步的JavaScipt 和 HTML，可以不刷新整个页面的情况下，对网页进行局部的更新。

## 实现方式

1. 原生的JS实现方式：**XMLHttpRequest** 

   * 创建XMLHttpRequest对象，根据浏览器版本不同，调用不同的方法。

   ```
   var xmlhttp;
   if (window.XMLHttpRequest)
     {// code for IE7+, Firefox, Chrome, Opera, Safari
     xmlhttp=new XMLHttpRequest();
     }
   else
     {// code for IE6, IE5
     xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
     }
   ```

   * 如果要将请求发送到服务器，用XMLHttpRequest对象的open方法。

   | 方法                         | 描述                                                         |
   | ---------------------------- | ------------------------------------------------------------ |
   | open(*method*,*url*,*async*) | method(get,post)，url：文件在服务器上的位置,async:同步异步   |
   | send(string)                 | 将请求发送到服务器，string参数仅用于post请求，将内容存到请求体中。 |

   ```
   //设置
   xmlhttp.open("get","AjaxServlet?name=admin",true);
   xmlhttp.send();
   ```

   * 相应 

   ```
   //等待服务器相应成功了再获取数据
   xmlhttp.onreadystatechange=function()
   {
   
   //ok了
       if (xmlhttp.readyState==4 && xmlhttp.status==200) 
       {
      	//获取服务器的响应结果	
       }
   }
   ```

2. JQuery实现方式：https://jquery.cuishifeng.cn/jQuery.Ajax.html

   * $.ajax(url,[setting])：通用方式

   除了url必须，其他seting都有默认或可以省略

   举例：
   
   ```
           $.ajax({
               url:"AjaxServlet",//请求url
               type:"POST",
               data:{"name":name,"age":23},//数据，传json对
               success:function(html){//成功相应的回调函数
                   $("#mes").html(html);
               },
               error:function(){
                   //如果请求出现404，就会执行此函数
               },
               dataType:JSON //设置收到相应的数据格式
           })
   ```
   
   * $.get()：发送get请求
   
   ```
   jQuery.get(url, [data], [callback请求成功时的回调函数], [type])
   
   $.get("AjaxServlet",{username:"fxw"},function(){},text)
   ```
   
   * $.post()
   
   ```
   jQuery.post(url, [data], [callback], [type])
   和上面差不多 
   ```
   
   



