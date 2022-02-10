## 官方文档

https://v3.bootcss.com/

## 概念

 一个前端开发的框架，Bootstrap，来自 Twitter，是目前很受欢迎的前端框架。Bootstrap 是基于 HTML、CSS、JavaScript 的，它简洁灵活，使得 Web 开发更加快捷。

* 框架:一个半成品软件，开发人员可以在框架基础上，在进行开发，简化编码。
* 好处：
	1. 定义了很多的css样式和js插件。我们开发人员直接可以使用这些样式和插件得到丰富的页面效果。
	2. 响应式布局。
		* 同一套页面可以兼容不同分辨率的设备。



## 基础模板

通过网络引用的地方，用本地引用

```
	 <!DOCTYPE html>
	<html lang="zh-CN">
	<head>
	    <meta charset="utf-8">
	    <meta http-equiv="X-UA-Compatible" content="IE=edge">
	    <meta name="viewport" content="width=device-width, initial-scale=1">
	    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
	    <title>Bootstrap HelloWorld</title>
	
	    <!-- Bootstrap -->
	    <link href="css/bootstrap.min.css" rel="stylesheet">
     <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
	    <script src="js/jquery-3.2.1.min.js"></script>
	    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
	    <script src="js/bootstrap.min.js"></script>
	</head>
	<body>
	<h1>你好，世界！</h1>
	
	</body>
	</html>
```



## 响应式布局

* 原理：依赖于Grid System 栅格系统

```
Bootstrap 包含了一个响应式的、移动设备优先的、不固定的网格系统，可以随着设备或视口大小的增加而适当地扩展到 12 列。它包含了用于简单的布局选项的预定义类，也包含了用于生成更多语义布局的功能强大的混合类。
```

### 实现步骤

1. 定义容器，相当于之前的table

   容器分类

   * container ：左右两边**留白**，每个设备会有固定宽度
   * container-fluid

2. 定义行row, 相当于\<tr>  

3. 定义元素。指定不同设备上，所占的格子数目。样式：.col-设备代号-格子数目

   设备代号

   * xs：小屏幕<768px
   * sm：平板>=768px
   * md：中等屏幕>=992px
   * lg：大屏幕>=1200px

```
<div class="container-fluid">
    <div class="row">
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>

        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>

        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>
        <div class="col-lg-1 col-sm-2 inner">栅格</div>

    </div>
</div>
```

