# 2. 准备及开发流程

## 1. 基于 Lavas 2.0 初始化一个项目，本实例中初始化项目名为 lavas-middleware

    具体步骤可参考 ![Lavas 2.0 中准备环境](https://lavas.baidu.com/codelab/get-started/prepare) 一节

    ``` bash

    npm install
    npm run dev

    ```
    起服务，查看 localhost:3000 可以看到如下图的一个初始化 Lavas 首页，恭喜你第一步已经完成啦！

    ![img](http://boscdn.bpc.baidu.com/assets/lavas/codelab/home-page.png)


## 2. 打开项目文件夹，我们发现根目录下有 `/middlewares` 的文件夹，我们自定义的中间件都要放在该文件下，对应生成的中间件名称就是'login'

``` js
lavas-middleware
    └── middlewares
           └── login.js

```

## 3. 写好中间件以后，还需要在 `lavas.config.js` 中完成配置，或是在具体的 `.vue` 文件中配置，才能生效哦！


> info
>
> 接下来我们就开始具体开发啦，让大家快速掌握 middleware 的开发和使用。

