

# 网络模块

项目中我们一般不直接用第三方的网络请求，因为第三方网络请求如果直接嵌到代码之中，坏处有两点。

1. 后期如果需要替换第三方网络请求需要改动大量代码
2. 在请求的时候容易造成代码冗余



所以我们需要对第三方网络请求进行封装，形成接口。在代码里只需要调用接口即可。



## 网络模块的选择

1. 基于XMLHttpRequest传统的ajax

为什么不推荐？

​	配置和调用使用混乱，编码看起来不清晰。

2. Query-ajax

Vue中为什么 不推荐？

​	因为Vue主要是数据驱动视图，不怎么需要手动修改dom，因此为了jqery-ajax导入jquery浪费大量的空间得不偿失。



推荐：axios

* axios是一个专注于发送http请求的库，比较小易于配置和封装。



# axios基本操作

## 拦截器

可以在数据请求和相应前进行处理(如请求前携带token令牌)

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```



# axios常用配置

## Vue全局配置

```js
//index.js
import axios from "axios"
axios.defaults.baseURL = "baseURL"
Vue.prototype.$http = axios;
```



## 基于axios的api封装

创建request.js 并且全局配置axios 对外共享

```js
//request.js
import axios from "axios"

export  const axios_escook = axios.create({
    baseURL: "https://www.escook.cn",
});
```



创建api目录 ，通过不同功能模块分类

![image-20220209001614542](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20220209001614542.png)

```
//Home.js
//向外共享对应的API返回的promise

//引入我们配置好的URL的路径
import { axios_escook } from "@/utils/request.js"


//向外导出API函数
export const getAriticleListAPI = function (_page, _limit) {
    return axios_escook.get("/articles", {
        params: {
            _page, _limit,
        }
    });
}



```

