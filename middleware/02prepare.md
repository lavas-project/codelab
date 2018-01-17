# 2. 准备及开发流程


## 简单总结以下几步：

（1）基于 Lavas 2.0 初始化一个项目，本示例中项目名为 lavas-middleware，具体步骤可参考 [Lavas 2.0 中准备环境](https://lavas.baidu.com/codelab/get-started/prepare) 一节

``` bash

npm install
npm run dev

```
起服务，查看 localhost:3000 可以得到一个初始化 Lavas 首页，恭喜你第一步已经完成啦！



（2）根目录下查看 `lavas.config.js` 文件，查看 router 配置项。

- 初始化项目默认是开启 SSR 模式，即页面首屏渲染为服务端渲染
- 若不想采用纯前端渲染，可以将这里设置为 false
- 本示例中的中间件支持服务端、浏览器端执行，为了测试两端的效果，这里设置 true 即可。页面首屏是采用服务端渲染，可测试服务端的中间件，此外前端跳转采用前端渲染，可测试浏览器端中间件。


``` js

module.exports = {
    router: {
        ssr: true, // 首屏服务端渲染
        mode: 'history',
        base: '/',
        pageTransition: {
            type: 'fade',
            transitionClass: 'fade'
        }
    }
};

```


（3）打开项目文件夹，我们发现根目录下有 `/middlewares` 文件夹，自定义的中间件都要放在该文件下，才可能被导入执行

（4）写好中间件以后，还需要在 `lavas.config.js` 中完成配置，或是在具体的 `.vue` 文件中配置，才能生效哦！注意：如下文件结构对应的中间件名称就是 `login`、`user/login`，供配置时使用


``` js
lavas-middleware
    └── middlewares
           └── login.js
           └── user
                └── login.js

```

> info
>
> 接下来我们就开始具体开发啦，让大家快速掌握 middleware 的开发和使用。

