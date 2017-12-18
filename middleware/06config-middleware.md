# 6. 配置 middleware

 ## 中间件需要通过配置后才能生效，我们提供了两种配置方式：

（1）全局性中间件在 `./lavas.config.js` 中配置，服务端执行还是前端执行，all 为前后端都执行，其中可以通过检测请求路径等方法来指定部分页面执行（不配置默认三个值都为空数，不执行中间件）

``` js

module.exports = {
    middleware: {
        all: [],
        server: ['login-server'],
        client: ['login-client']
    }
};


```

（2）组件级别中间件可以在单个 `.vue` 文件中单独配置 middleware 属性，注意：这里配置的中间件，前后端都会执行

``` js

// .vue文件
export default {
    name: 'name',
    middleware: ['middleware-1', 'middleware-2'],
    ...
};


```

> info
>
> 本示例中只配置了第一种全局方式，具体代码如上
>
> 下一节我们可以起服务进行具体的测试、验证啦

