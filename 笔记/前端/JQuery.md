# JQuery

## 入门

* 概念：JavaScript的框架，简化js的开发,本质上就是js文件，封装了很多方法。
* 快速入门：
* $("#id名") 快速获取组件对象：\$就是一个方法 

```
<body>
    <div id="div1">div1..</div>
    <div id="div2">div2..</div>
    <script >
        let div=$("#div1")
        alert(div.html()); //注意这里不能用innerHTML
    </script>
</body>
```

* JQuery对象和js对象的转换：这俩个获取的对象是有部分区别的，但是可以转换

```
       // 1.通过js方式，获取所有名称叫div的对象
       var div =document.getElementsByTagName("div");
       // 2.通过JQuery方式，获取所有名称叫div的对象
       var $div =$("div");
       //都可以当数组使用
       alert(div.length);
       alert($div.length);
       //把每个元素修改为aaa 用js的方式
       for(var i=0; i<div.length;i++){
           // div[i].innerHTML="sb";
           $(div[i]).html("ccc");//转换
       }
       //用JQuery的方式
       // $div.html("bbb");

       //JQuery对象在操作时更加方便，js和Jquery方法是不通用的
       //1. 互相转换 Js->JQ $(js对象) 
       for(var i=0; i<div.length;i++){
           // div[i].innerHTML="sb";
           $(div[i]).html("ccc");//转换
       }

       // 2. JQ->Js 用索引访问 就可以变成Js
       $div[0].innerHTML="dd";
       $div.get(1).innerHTML="zzzz";
```

* 互相转换

  1. Js->JQ ：   $(js对象) 

     2. JQ->Js：   对象名[索引]  因为JQ可以当成数组使用，用索引访问可以转化成js。、



### 事件绑定

* 给b1按钮绑定事件

js实现

```
    <input type="button" value="点我" id="b1">                 
    <script>                                                 
        // js                                                
        var elementById = document.getElementById("b1");     
        elementById.onclick=function (){                     
            alert("触发");                                     
        }                                                    
```

jq实现

```
        $("#b1").click(function (){           
            alert("触发了");                     
        })                                                                  
```

* 实现详情：参数里面传一个匿名内部类



### 入口函数

* 入口函数： dom文档加载完以后，就会执行入口函数的代码

* jQuery入口函数可以书写多次，window.onload只能书写一次，定义多次就会被覆盖掉。
* 执行的时机不同，jQuery的入口，一旦dom结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成；window.onload 的是等页面文档、外部的js文件、css文件、图片加载完毕才执行内部代码。

```
    <script>
        //js 入口函数——页面加载完以后调用而且只会调用一次
        window.onload=function (){
              $("#b1").click(function (){
                  alert("触发了");
              })
        };

        //js 入口函数 dom文档加载完以后，就会执行入口函数的代码
        $(function (){
            alert("第一次");
              $("#b1").click(function (){
                  alert("触发了");
              })
        })

        // 可以调用多次
        $(function (){
            alert("第二次");
        })

    </script>

```





## 控制CSS

* 语法：对象.css("css属性名"，“值”)；

```
        $(function (){
              $("#b1").click(function (){
                  $div1.css("backgroundColor","red");
              })
        })
```



## 选择器



## 基本选择器

1. 基本选择器

   1. **标签选择器**（元素选择器）

      	* 语法： $("html标签名") 获得所有匹配标签名称的元素
      	2. id选择器 
      		* 语法： $("#id的属性值") 获得与指定id属性值匹配的元素
      	3. 类选择器
      		* 语法： $(".class的属性值") 获得与指定的class属性值匹配的元素
      	4. 并集选择器：
      		* 语法： $("选择器1,选择器2....") 获取多个选择器选中的所有元素

      2. **层级选择器**

         1. 后代选择器 (子孙 都会包括进来)
            * 语法： $("A B ") 选择A元素内部的所有B元素		
         2. 子选择器 (只选择儿子)
            * 语法： $("A > B") 选择A元素内部的所有B子元素

         ```
         $("body div").css("backgroundColor","red");//把body里面的所有div都变成红色
         ```

      3. **属性选择器**

        4. 属性名称选择器 

        	* 语法： $("A[xx属性名]") 有xx属性
        	
        	2. 属性选择器
        	
        	* 语法： $("A[xx='aa']") 有xx属性 并且值为aa（不等于+ ！）
        	
        	3. 复合属性选择器
        	
        	* 语法： $("A\[属性名='值']\[]...") 包含多个属性条件的选择器

      | 符号  | 作用      |
      | ----- | --------- |
      | ^=’x' | 以x开头   |
      | !='x' | 不等于x   |
      | $='x' | 以x为结尾 |
      | =‘x’  | 等于x     |
      | *=’x' | 含有x     |

        * 样例：

          ```
          			$("#b1").click(function (){
          				$("div[title]").css("backgroundColor","red");
          			})
          
          			// <input type="button" value=" 属性title值等于test的div元素背景色为红色"  id="b2"/>
          			$("#b2").click(function (){
          				$("div[title='test']").css("backgroundColor","red");
          			})
          
          			// <input type="button" value=" 属性title值不等于test的div元素(没有属性title的也将被选中)背景色为红色"  id="b3"/>
          			$("#b3").click(function (){
          				$("div[title!='test']").css("backgroundColor","red");
          			})
          
          			// <input type="button" value=" 属性title值 以te开始 的div元素背景色为红色"  id="b4"/>
          			$("#b4").click(function (){
          				$("div[title='tet01']").css("backgroundColor","red");
          			})
          			// <input type="button" value=" 属性title值 以est结束 的div元素背景色为红色"  id="b5"/>
          			$("#b5").click(function (){
          				$("div[title$='est']").css("backgroundColor","red");
          			})
          
          			// <input type="button" value="属性title值 含有es的div元素背景色为红色"  id="b6"/>
          			$("#b6").click(function (){
          				$("div[title*='es']").css("backgroundColor","red");
          			})
          
          			// <input type="button" value="选取有属性id的div元素，然后在结果中选取属性title值含有“es”的 div 元素背景色为红色"  id="b7"/>
          			$("#b7").click(function (){
          				$("div[title*='es'][id]").css("backgroundColor","red");
          			})
          ```

      4. **过滤选择器**

        5. 首元素选择器 

        	* 语法： :first 获得选择的元素中的第一个元素
        	
        	2. 尾元素选择器 
        	
        	* 语法： :last 获得选择的元素中的最后一个元素
        	
        	3. 非元素选择器
        	
        	* 语法： :not(selector) 不包括指定内容的元素
        	
        	4. 偶数选择器
        	
        	* 语法： :even 偶数，从 0 开始计数
        	
        	5. 奇数选择器
        	
        	* 语法： :odd 奇数，从 0 开始计数
        	
        	6. 等于索引选择器
        	
        	* 语法： :eq(index) 指定索引元素
        	
        	7. 大于索引选择器 
        	
        	* 语法： :gt(index) 大于指定索引元素
        	
        	8. 小于索引选择器 
        	
        	* 语法： :lt(index) 小于指定索引元素
        	
        	9. 标题选择器
        	
        	* 语法： :header 获得标题（h1~h6）元素，固定写法

        ```
      		$(function (){
      			// <input type="button" value=" 改变第一个 div 元素的背景色为 红色" id="b1"/>
      			$("#b1").click(function (){
      				$("div:first").css("backgroundColor","red")
      			});
      
      			// <input type="button" value=" 改变最后一个 div 元素的背景色为 红色" id="b2"/>
      			$("#b2").click(function (){
      			})
      			// <input type="button" value=" 改变class不为 one 的所有 div 元素的背景色为 红色" id="b3"/>
      			$("#b3").click(function (){
      				// $("div[class!='one']").css("backgroundColor","red"); 这个也可以
      				$("div:not(.one)").css("backgroundColor","red");
      			})
      			// <input type="button" value=" 改变索引值为偶数的 div 元素的背景色为 红色" id="b4"/>
      			$("#b4").click(function (){
      				$("div:even").css("backgroundColor","red")
      			})
      			// <input type="button" value=" 改变索引值为奇数的 div 元素的背景色为 红色" id="b5"/>
      			$("#b5").click(function (){
      				$("div:odd").css("backgroundColor","red")
      			})
      			// <input type="button" value=" 改变索引值为大于 3 的 div 元素的背景色为 红色" id="b6"/>
      			$("#b6").click(function (){
      				$("div:gt(3)").css("backgroundColor","red")
      			})
      			// <input type="button" value=" 改变索引值为等于 3 的 div 元素的背景色为 红色" id="b7"/>
      			$("#b7").click(function (){
      				$("div:eq(3)").css("backgroundColor","red")
      			})
      			// <input type="button" value=" 改变索引值为小于 3 的 div 元素的背景色为 红色" id="b8"/>
      			$("#b8").click(function (){
      				$("div:lt(3)").css("backgroundColor","red")
      			})
      			// <input type="button" value=" 改变所有的标题元素的背景色为 红色" id="b9"/>
      			$("#b9").click(function (){
      				$(":header").css("backgroundColor","red")
      				$("h1:header").css("backgroundColor","red")
      				$("h2:header").css("backgroundColor","red")
      			})
      		})
        ```

      5. **表单过滤选择器**

        6. 可用元素选择器 

        	* 语法： :enabled 获得可用元素
        	
        	2. 不可用元素选择器 
        	
        	* 语法： :disabled 获得不可用元素
        	
        	3. 选中选择器 
        	
        	* 语法： :checked 获得单选/复选框选中的元素
        	
        	4. 选中选择器 
        	
        	* 语法： :selected 获得下拉框选中的元素

        ```
      			// <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内可用 <input> 元素的值" id="b1"/>
      			$("#b1").click(function (){
      				$("input:enabled").val(666);
      			})
      			// <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内不可用 <input> 元素的值" id="b2"/>
      			$("#b2").click(function (){
      				$("form input:disabled").val(666);
      			})
      			// <input type="button" value=" 利用 jQuery 对象的 length 属性获取复选框选中的个数" id="b3"/>
      			$("#b3").click(function (){
      				alert($("input[type='checkbox']:checked").length);
      			})
      
      			// <input type="button" value=" 利用 jQuery 对象的 length 属性获取下拉框选中的个数" id="b4"/>
      			$("#b4").click(function (){
      				alert($("select option:selected").length);
      			})
        ```

        

## JQuery DOM操作

1. 内容操作
   	1. html(): 获取/设置元素的标签体内容   \<a>\<font>内容</font></a>  --> <font>内容</font>
        	2. text(): 获取/设置元素的标签体纯文本内容   \<a>\<font>内容</font></a> --> 内容
           	3. val()： 获取/设置元素的value属性值

   ```
   		<script>
   			$(function(){
   				// 获取myinput 的value值
   				var val = $("#myinput").val();
   				// 设置value的值
   				var val2 = $("#myinput").val("66");
   
   				// 获取mydiv的标签体的内容 
   				alert($("#mydiv").html() );  //输出<p><a href="#">标题标签</a></p>
   
   				// 获取mydiv文本内容
   				alert($("#mydiv").text() );
   			})
   		</script>
   	</head>
   	<body>
   		<input id="myinput" type="text" name="username" value="张三" /><br />
   		<div id="mydiv"><p><a href="#">标题标签</a></p></div>
   	</body>
   ```

2. 属性操作

  3. 通用属性操作

   4. attr(): 获取/设置元素的属性

   5. removeAttr():删除属性

   6. prop():获取/设置元素的属性

   7. removeProp():删除属性

  	* attr和prop区别？
  		1. 如果操作的是元素的固有属性，则建议使用prop（check ed,selected里面就用attr就获取不到了）
  		2. 如果操作的是元素自定义的属性，则建议使用attr


    1. 对class属性操作
       1. addClass():添加class属性值
          2. removeClass():删除class属性值
             3. toggleClass():切换class属性
    
    	* toggleClass("one"): 
    		* 判断如果元素对象上存在class="one"，则将属性值one删除掉。  如果元素对象上不存在class="one"，则添加
    4. css(): 上面有

## CRUD

 CRUD操作:

    1. append():父元素将子元素追加到末尾
    
           1. append(对象2): 将对象2添加到对象1元素内部，并且在末尾
                  2. prepend():父元素将子元素追加到开头
    
    	* 对象1.prepend(对象2):将对象2添加到对象1元素内部，并且在开头
    3. appendTo(): //**反过来**
    	* 对象1.appendTo(对象2):将对象1添加到对象2内部，并且在末尾
    4. prependTo()：
    	* 对象1	.prependTo(对象2):将对象1添加到对象2内部，并且在开头
    5. . after():添加元素到元素后边
         * 对象1.after(对象2)： 将对象2添加到对象1后边。对象1和对象2是兄弟关系
      6. before():添加元素到元素前边 。
         * 对象1.before(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系
    7. insertAfter() 
      * 对象1.insertAfter(对象2)：将对象2添加到对象1后边。对象1和对象2是兄弟关系
    8. insertBefore()
      * 对象1.insertBefore(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系
    9. remove():移除元素
       * 对象.remove():将对象删除掉
    10. empty():清空元素的所有**后代**元素。本身不清除，但是后代会被全部清除掉。
        * 对象.empty():将对象的后代元素全部清空，但是保留当前对象以及其属性节点

