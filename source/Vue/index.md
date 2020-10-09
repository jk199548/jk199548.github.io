---
title: vue
date: 2020-09-27 17:30:42
---
[参考链接](https://juejin.im/post/6844903632815521799)
## 关于vue项目中的一些常见问题 
#### 1、路由传参问题(query和params+动态路由传参)

```
1、query的传参
<router-link :to="{path: 'detail', query: {id: 1}}">前往detail页面</router-link>
    1)query跳转的路径key值为path，
    2)query通过this.$route.query.id来接收参数
    3)query传参的url展现方式：/detail?id=1&user=123&identity=1

2、params+动态路由传参
<router-link :to="{name: 'detail', params: {id: 1}}">前往detail页面</router-link>
    1)params通过this.$route.params.id来接收参数
    2)params＋动态路由的url方式：/detail/123
    3)params动态路由传参，一定要在路由中定义参数，然后在路由跳转的时候必须要加上参数，否则就是空白页面：
    {      
      path: '/detail/:id',      
      name: 'Detail',      
      component: Detail    
    }
3、跨域问题
  proxyTable: {
    // 用‘/api’开头，代理所有请求到目标服务器
    '/api': {
      target: 'http://jsonplaceholder.typicode.com', // 接口域名
      changeOrigin: true, // 是否启用跨域
      pathRewrite: { //
        '^/api': ''
      }
    }
  }
```