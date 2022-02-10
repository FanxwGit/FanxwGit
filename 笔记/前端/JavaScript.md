# 语法基础

## 转换



* 数字转String
  * toString()
* String转数字
  * Number('String')

## 函数

* 函数声明：

  ```javascript
  function 函数名(变量){
  }
  ```

* 参数:

  * 实参个数小于形参个数，多于的形参，定义成undefined
  * 实参个数多于形参个数，只取形参的个数
  
* 可以在函数里再声明函数

* 函数变量:

  ```
  var f = function (){
  }
  f();
  ```

## JS作用域

### JS没有块级作用域

也就是说，在**循环**，**if**里面声明的变量也是全局变量。

```js
if(){
	var a=1;   
}
console.log(a);
```

## 预解析

```
var a=1;
```

这行简单的代码其实是两个步骤：声明和定义。

- **声明**：var num; 告诉浏览器在全局作用域中有一个num变量了，如果一个变量只是声明了，但是没有赋值，默认值是undefined。
- **定义**：num = 12; 定义就是给变量进行赋值。

#### var声明的变量和function声明的函数在预解析的区别

var声明的变量，只是告诉全局作用域有这个变量，并不会进行赋值，而function函数会在预解析的时候同时进行**声明**和**定义**。

```js
f();
function f{
    console.log(1);
}
```



#### 样例

```
function f(){
	var a=b=c=1;//实际上等于 var a=1,b=1,c=1;因此b c 是全局.a是局部变量
}
console.log(a);//not define
console.log(b);//9
console.log(c);//9


```

