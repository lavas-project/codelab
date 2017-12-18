# 3. 添加中间件文件

## 中间件功能

读取 store 中保存的登录状态，如果登录了就跳过，未登录跳转到登录页面，点击登录即可完成模拟登录，这节主要是编写中间件代码，浏览器端和服务器端的分两部分介绍，如果您在中间件中的具体操作处理前后端完全一致，可以写成一个。具体文件结构和代码如下：

``` js

lavas-middleware
    └── middlewares
           └── login-server.js
           └── login-client.js

```


（1）添加浏览器端中间件 `./middlewares/login-client.js`，中间件的输入参数，可以在 `./core/middleware.js` 里的 getClientContext() 方法中查看或扩展 ctx ；

``` js

// login-client.js
export default function ({store, redirect, route}) {

    // 查看 store 中的登陆状态，前端路由跳转不会重置 store 中的值
    let isLogin = store.state.common.login || false;

    // 如果未登录且当前路由不是到登录页，则重定向到登录页
    // 通过 route.path 判断路由
    if (!isLogin && route.path !== '/login') {
        return redirect({
            path: '/login'
        });
    }
}

```

（2）添加服务器端中间件 `./middlewares/login-server.js`，中间件的输入参数，可以在 `./core/middleware.js` 里的 getServerContext() 方法中查看或扩展 ctx ；

``` js

// login-server.js
export default async function ({store, redirect, req}) {

    // 如果是服务端，store 中都是初始化的状态
    // 需要先去通过接口向服务器去验证用户是否登陆，并设置 store 中的登陆状态，再进行检测
    await store.dispatch('/common/validLogin');

    let isLogin = store.state.common.login || false;

    // 如果未登录且当前路由不是到登录页，则重定向到登录页
    // 通过 req.path 判断路由
    if (!isLogin && req.path !== '/login') {
        return redirect({
            path: '/login'
        });
    }
}

```


> info
>
> 两个中间件添加已经完成啦
>
> 下一节我们将添加一个简单的登录页面，满足重定向需求


