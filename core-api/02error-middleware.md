# 2. 自定义错误处理中间件

在本节中，我们将使用自定义的错误处理中间件替换掉 Lavas 内置的中间件。

## 创建项目

首先使用 `lavas init` 创建一个 Lavas Basic 模版项目，默认开启 SSR 模式。

在 `server.dev.js` 中，我们使用全部 Lavas 内置的中间件，其中就包括了处理错误的中间件。
```javascript
// server.dev.js

// 使用全部内置中间件
app.use(core.expressMiddleware());
```

> info
>
> 本教程以 express 为例，如果您想使用 Koa，可以参考[这篇教程](/codelab/koa/01introduction)。

## 选取部分中间件

要进行内置中间件的替换，首先需要选取部分 Lavas 内置中间件：
* 处理静态文件的 `static` 中间件
* 进行服务端渲染的 `ssr` 中间件
```javascript
// server.dev.js

app.use(core.expressMiddleware([
    'static',
    'ssr',
    // 'error' // 不使用错误处理中间件
]));
```

> info
>
> 如果您想了解 Lavas 内置的全部中间件，可以参考[这篇文档](/guide/v2/advanced/core-api#lavas-%E5%86%85%E7%BD%AE%E4%B8%AD%E9%97%B4%E4%BB%B6)。

## 自定义错误中间件

在 Lavas 内置中间件之后，我们定义一个简单的错误处理中间件，其中简单的打印错误堆栈并返回 `500` 状态码：
```javascript
// server.dev.js

// 自定义错误中间件
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Internal Server Error');
});
```

## 测试效果

为了测试效果，我们可以在 `core/entry-server.js` 中尝试访问 Node 环境中不存在的 `window` 对象：
```javascript
// core/entry-server.js

export default function (context) {
    return new Promise((resolve, reject) => {
        // 访问并不存在的 window 对象
        window;
```

使用 `lavas dev` 启动开发服务器后，访问页面，将看到控制台打印出 `ReferenceError: window is not defined` 信息，而浏览器得到 500 响应。
