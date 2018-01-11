# 3. 自定义 SSR 中间件

在上一节我们自定义了一个简单的错误处理中间件，本节将使用 Lavas API 提供的 `render()` 方法自定义服务端渲染中间件。

## 选取 Lavas 内置中间件

错误中间件必须放在最后，因此我们的 SSR 中间件需要放在中间：
```javascript
app.use(core.expressMiddleware(['static', 'favicon']));

// 自定义 SSR 中间件
app.use((req, res, next) => {});

// 错误处理中间件放在最后
app.use(core.expressMiddleware('error'));
```

## 自定义 SSR 中间件

下面我们定义一个简单的 SSR 中间件。Lavas API 提供了 `render()` 方法，接收上下文对象作为参数，交给 Vue bundleRenderer 得到渲染结果，结果中包含了 `err` 和 `html` 两部分：
```javascript
app.use((req, res, next) => {
    core.render({
        url: req.url
    }).then(result => {
        if (result.err) {
            return next(result.err);
        }
        res.end(result.html);
    });
});
```
