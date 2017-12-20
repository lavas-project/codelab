# 3. 使用 Koa 作为开发服务器

之前已经完成了环境的准备工作，在本节中，我们将演示如何使用 Koa 作为服务器。

## 修改开发服务器脚本

打开开发服务器脚本 `server.dev.js`，首先引入 Koa，替换掉 express：
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

## 启动开发服务器

启动开发服务器后，将看到 express 已经切换成了 Koa，一切就是这么简单！
```bash
lavas dev
```

## 修改 SSR 模式下线上服务器

在 SSR 模式下，如果在线上服务器脚本 `server.prod.js` 也想使用 Koa，应该怎么做呢？参考上述开发服务器的修改，相信您将很容易实现。
