# 3. 添加中间件文件

## 中间件功能

- 这节主要是编写中间件代码，浏览器端和服务器端的分两部分介绍，如果中间件中两端处理完全一致，可以写成一个，配置成前后端均执行即可
- 浏览器端：读取 store 中保存的登录状态，如果已登录则跳过，未登录跳转到登录页面
- 服务器端：请求服务器验证登录状态，并设置在 store 中的登录状态，如果验证登录则跳过，未登录则重定向到登录页面

具体文件结构和代码如下：

``` js

lavas-middleware
    └── middlewares
           └── login-server.js
           └── login-client.js

```


（1）添加浏览器端中间件 `./middlewares/login-client.js`，中间件的输入参数，可以在 `./.lavas/middleware.js` 里的 getClientContext() 方法中查看。如果需要扩展参数，可以单独在 `./core/entry-server.js`、 `./core/entry-client.js` 中引入自定义的文件（注意只需基于 `./.lavas/middleware.js` 文件，在 `getClientContext` 方法中添加即可）；

``` js

// login-client.js
export default function ({store, redirect, route}) {

    // 查看 store 中的登录状态，前端路由跳转不会重置 store 中的值
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

（2）添加服务器端中间件 `./middlewares/login-server.js`，中间件的输入参数，可以在 `./.lavas/middleware.js` 里的 getServerContext() 方法中查看。如果需要扩展参数，可以单独在 `./core/entry-server.js`、 `./core/entry-client.js` 中引入自定义的文件（注意只需基于 `./.lavas/middleware.js` 文件，在 `getServerContext` 方法中添加即可）；

``` js

// login-server.js
export default async function ({store, redirect, req}) {

    // 如果是服务端，store 中都是初始化的状态
    // 需要先去通过接口向服务器去验证用户是否登录，并设置 store 中的登录状态，再进行检测
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


