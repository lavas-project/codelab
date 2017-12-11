# 12. 使用 Koa (扩展)

由于 Koa 使用了 ES7 async/await 语法，很多基于 Koa 的中间件都是直接使用，并没有使用类似 Babel 进行转译。所以 Koa 对 Node.js 的版本是有要求的，必须高于 7.6.0。

我们以开发服务脚本为例，打开 `server.dev.js`，首先引入 Koa：
```
- const express = require('express');
+ const Koa = require('koa');
```

然后使用 Koa 创建 `app`，并使用 `koaMiddleware` 代替 `expressMiddleware`：
```
function startDevServer() {
    - app = express();
    + app = new Koa();
    core.build()
        .then(() => {
            - app.use(core.expressMiddleware());
         + app.use(core.koaMiddleware());
```

通过 `lavas dev` 启动开发服务器，将看到 express 已经切换成了 Koa，一切就是这么简单！
