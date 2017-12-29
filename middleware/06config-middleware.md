# 6. 配置 middleware

 ## 中间件需要通过配置后才能生效，我们提供了两种配置方式：

（1）全局性中间件在 `./lavas.config.js` 中配置，提供三个值：server —— 服务端执行、client —— 前端执行，all —— 前后端均执行（不配置默认三个值都为空数组，不执行全局中间件）。其中具体的中间件可以通过执行时检测请求路径等方法来指定部分页面执行，如示例中间件会检测页面路径，避免登录页面的重定向。


``` js

module.exports = {
    middleware: {
        all: [], // 前后端均执行
        server: ['login-server'], // 仅服务器端执行
        client: ['login-client']  // 仅浏览器端执行
    }
};


```


（2）组件级别中间件可以在单个 `.vue` 文件中单独配置 middleware 属性，注意：这里配置的中间件，前后端均会执行。

``` js

// .vue文件
export default {
    name: 'name',
    middleware: ['middleware-1', 'middleware-2'],
    ...
};


```

如果您想更深入的了解前后端中间件的导入执行顺序可分别在 `./core/entry-client.js` 、`./core/entry-server.js` 中查看。

> info
>
> 本示例中只配置了第一种全局方式，具体代码如上（1）所示
>
> 下一节我们可以进行具体的测试、验证啦

