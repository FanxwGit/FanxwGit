## JSON介绍

JSON: **J**ava**S**cript **O**bject **N**otation(JavaScript 对象表示法)

是存储和交换文本信息的语法，有点类似XML,不过它更小更好解析。

* JSON实例：

```
var json={"key":"value"}
var json={key:"value"} //键可以不带双引号
```





## JSON和JS对象的转换

JSON 文本格式在语法上与创建 JavaScript 对象的代码相同。

由于这种相似性，无需解析器，JavaScript 程序能够使用内建的 eval() 函数，用 JSON 数据来生成原生的 JavaScript 对象。





## JSON语法

JSON 语法是 JavaScript 对象表示语法的子集。

* 声明

每个数据由都逗号分割，每个数据都有键和值

```
json={"key":value,"key":[list1]}
```

* 访问

可以通过.键值 或者 索引访问

```
json["key"];
json.key
```

* 删除

通过 delete 删除json里面的元素

```
delete json.key;
console.log(json.key);
//结果是undefined
```



## jSON.parse()

在接收服务器的数据时候一般是字符串

可以用parse将字符串解析成json数据格式

* 语法

```
JSON.parse(text,[reviver])
/*
text:必需， 一个有效的 JSON 字符串。
reviver: 可选，一个转换结果的函数， 将为对象的每个成员调用此函数，具体百度。
*/
```

```
var json = JSON.parse('{ "name":"fxwnb"}');
```

## JSON.stringify()

给服务器发发数据一般是丢字符串过去

用stringify将我们的json转成字符串格式

由于JSON不允许包括函数和Date对象，因此要先进行转化

* 实例

```
var obj = { "name":"Runoob", "alexa":function () {return 10000;}, "site":"www.runoob.com"};
obj.alexa = obj.alexa.toString();//先把function转化成字符串
var myJSON = JSON.stringify(obj);
```



## JAVA对象和JSON类型的转换

- [Gson](https://github.com/google/gson): 谷歌开发的 JSON 库，功能十分全面。
- [FastJson](https://github.com/alibaba/fastjson): 阿里巴巴开发的 JSON 库，性能十分优秀。
- [Jackson](https://github.com/FasterXML/jackson): 社区十分活跃且更新速度很快。



下面是jackson的笔记



*  环境配置

```
        <!--Jackson包-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.3</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.9.3</version>
        </dependency>
```



常用方法介绍

```
writeValueAsString(Map<>)//把map丢进去 转成string
writeValue(字符输出流和字节输出流都可以，map)；//把流丢进去 map丢进去，自动会帮你调用流的输出，并且把map转成json格式
```





* 实战

判断用户是否已存在

![image-20210921152018096](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210921152018096.png)

* 前端

思路是，当输入失去焦点了调用函数，函数里发送ajax请求给服务器。

服务器给前端丢json数据，然后再解析即可。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="jquery-3.2.1.min.js"></script>
    <script>
        $(function (){
            $("#name").blur(function(){
                let mes={"username":$("#name").val()};
                $.get("HelloJSONServlet",
                    mes,//附带信息
                    function (data){
                        console.log(data);
                        let mes=$("#mes");
                        if(data.usernameExit == 1){
                            mes.css("color","red");
                        }
                        else{
                            mes.css("color","green");
                        }
                        mes.html(data["mes"]);
                    },"json");
            });
        })
    </script>

</head>

<body>
<input type="text" id="name">
<span id="mes">请输入用户名</span>
</body>

</html>
```





* 后端

```
ublic class HelloJSONServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        boolean usernameExit="fxw".equals(username);
        String mes;
        if(usernameExit){
            mes="此id已被注册";
        }
        else{
            mes="恭喜你，此用户名可用";
        }

        response.setContentType("text/html;charset=utf-8");
        HashMap<String,Object>map=new HashMap<>();
        map.put("usernameExit",usernameExit);
        map.put("mes",mes);
        ObjectMapper objectMapper=new ObjectMapper();

        //以下两种都可以
        String json = objectMapper.writeValueAsString(map);//把map转成string模式
        response.getWriter().write(json);//再传回客户端

        //objectMapper.writeValue(response.getWriter(),map);把流丢进去 map丢进去，自动会帮你调用流的输出，并且把map转成json格式
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }

}
```



