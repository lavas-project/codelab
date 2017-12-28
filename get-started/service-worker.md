# 8. 编写 Service Worker

这一部分我们只列出一些最基本的 Service Worker 的配置方法，有关 Service Worker 更详细的介绍可以移步 Lavas 官网的[什么是 Service Worker](https://lavas.baidu.com/doc/offline-and-cache-loading/service-worker/service-worker-introduction) 和 Lavas 文档的 [Service Worker 部分](/guide/v2/advanced/service-worker)。此外由于 App Shell 模型和骨架屏 (Skeleton) 比较复杂，我们单独编写了一套 [Codelab](/codelab/appshell/introduction) 来介绍，这里就不再涉及了。

初始化生成的项目默认已经带有 Service Worker，我们来查看一下加深了解。Lavas 的 Service Worker 可以分为两部分：__配置部分__和__模板部分__。

## Service Worker 配置项

首先打开根目录下的 `/lavas.config.js`，关注 `serviceWorker` 这一段，如下：

```javascript
module.exports = {
    // ...
    serviceWorker: {
        swSrc: path.join(__dirname, 'core/service-worker.js'),
        swDest: path.join(BUILD_PATH, 'service-worker.js'),
        globDirectory: path.basename(BUILD_PATH),
        globPatterns: [
            '**/*.{html,js,css,eot,svg,ttf,woff}'
        ],
        globIgnores: [
            'sw-register.js',
            '**/*.map'
        ],
        appshellUrls: ['/appshell'],
        dontCacheBustUrlsMatching: /\.\w{8}\./
    },
    // ...
};
```

这些配置项都是必填项，具体每一项的含义和配置方法可以参考 Lavas 文档的 [Service Worker 部分](/guide/v2/advanced/service-worker)。大致来说，这里配置了 Service Worker 模板所在位置，最终 js 生成位置，预缓存静态文件的覆盖范围等等。如上的配置将 `html`, `js`, `css`, `eot`, `svg`, `ttf`, `woff` 等各类静态资源加入预缓存文件，但是将 `sw-register.js` 和 `map` 排除在外。

`appshellUrls` 配置项和 App Shell 有关，这里先忽略。

## Service Worker 模板

通过 Service Worker 配置项可以列出预缓存文件列表。如果需要设置动态缓存和 appshell，就需要使用 Service Worker 模板了，位于 `/config/service-worker.js`。

```javascript
importScripts('/static/js/workbox-sw.prod.v2.1.2.js');

const workboxSW = new WorkboxSW({
    cacheId: 'lavas-cache',
    ignoreUrlParametersMatching: [/^utm_/],
    skipWaiting: true,
    clientsClaim: true
});

// Define precache injection point.
workboxSW.precache([]);

// Define response for HTML request.
workboxSW.router.registerNavigationRoute('/appshell');
```

默认情况下，Service Worker 的模板为我们完成了：

1. 引入 WorkBox 的 API
2. 初始化 WorkBox 实例
3. 定义预缓存文件列表注入入口
4. 注册 App Shell 路由规则

关于这些细节的内容在 Lavas 文档的 [Service Worker 部分](/guide/v2/advanced/service-worker)中会有详细介绍。这里我们暂且把这些认定为固定内容，直接往文件末尾继续追加动态缓存的配置即可。在之前的小节中，我们访问了雅虎天气的接口获取上海的天气。因此我们尝试将它缓存起来。

```javascript
// Define runtime cache.
workboxSW.router.registerRoute(new RegExp('https://query\.yahooapis\.com/v1/public/yql'),
    workboxSW.strategies.networkFirst());
```

我们为雅虎天气的 URL 设定了 networkFirst 缓存策略，在网络正常时会请求网络并更新缓存；否则会使用缓存内容。配合预缓存了的所有静态文件，站点就拥有了离线访问能力！
