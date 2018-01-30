# 6. 代理 API 请求

在前后端分离的开发模式中，后端数据接口服务往往不在本机，而前端在开发的时候需要使用到这些数据，那么请求跨域了，有很多办法可以帮助解决这个跨域的问题，可以通过 nginx 代理，实现 CORS 等，这些方法不是需要后端配合就是代价相对较高。

在这里，Lavas 的方案中默认提供了一个代理 API 的能力，类似于 [Vue Template ProxyTable](https://vuejs-templates.github.io/webpack/proxy.html)，下面我们来看看如何使用 proxyTable 完成代理的工作。

Lavas 方案中，开发模式下 proxyTable 的实现全部在 `server.dev.js` 文件中。

## 添加 proxyTable 配置

找到 `const proxyTable`，增加 proxyTable 的配置，如下面的代码所示。

```javascript
const proxyTable = {
    '/api': {
        target: 'https://xxx.xxx',
        changeOrigin: true
    }
};
```

重新启动开发服务器 `lavas dev`，启动完成之后，访问 `http://localhost:3000/api/` 将会被代理到 `target` 服务器。

以上配置就完成了 proxyTable 的功能。

## proxyTable 详细配置

Lavas 方案中 proxyTable 的实现是采用的 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)，详细的配置可以参考它的文档。

