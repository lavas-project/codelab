# 5. PWA 相关迁移

Lavas 和 PWA 相关的功能主要有两部分： manifest 和 Service Worker。我们逐个来看。

## manifest

Lavas 1.0 中，理论上 `manifest.json` 可以放在任何位置，并在 `/index.html` 中使用 `<link rel="manifest" href="some/path/to/manifest.json">` 进行引用。Lavas 初始化的项目将其放在 `/static/` 目录中。

Lavas 2.0 中，`/index.html` 已经升级成为 `/core/index.html.tmpl`，并且注册 manifest.json 的工作由 Lavas 自动生成，因此要求用户 __必须__ 将其放在 `/static/manifest.json`，并且__不得更名__。

manifest.json 的写法在两个版本中没有差别，因此可以直接复制到对应位置。

## Service Worker

Service Worker 的情况比起 manifest.json 复杂得多。Lavas 1.0 使用 sw-toolbox 和 sw-precache 生成 Service Worker。相关的文件包括：

* `/config/sw-precache.js`
* `/config/sw.tmpl.js`
* `/src/sw-register.js`

一般情况，我们会通过修改 `/config/sw-precache.js` 来定义预缓存文件列表 (`staticFileGlobs`) 和动态缓存 (`runningCache`)。其他配置项和另外两个文件绝大多数情况不会修改。

Lavas 2.0 使用了 [WorkBox](https://github.com/GoogleChrome/workbox) 生成 Service Worker，因此写法也变化很大。详细的信息可以参考[Lavas 文档](/guide/v2/advanced/service-worker)，这里仅列出我们关心的基本改动点。

### 预缓存文件列表

修改 Lavas 2.0 的 `/lavas.config.js` 中的 `serviceWorker` 对象，如下：

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
        dontCacheBustUrlsMatching: /\.\w{8}\./
    },
    // ...
};
```

关注 `globDirectory`, `globPatterns` 和 `globIgnores` 三个配置项。它们将 `html`, `js`, `css`, `eot`, `svg`, `ttf`, `woff` 等各类静态资源加入预缓存文件，但是将 `sw-register.js` 和 `map` 排除在外。

### 动态缓存

Lavas 1.0 通过在 `/config/sw-precache.js` 中定义 `runningCache` 实现动态缓存，如配置：

```javascript
{
    urlPattern: /\/fonts\//,
    handler: 'networkFirst',
    options: {
        cache: {
            maxEntries: 10,
            name: 'fonts-cache'
        }
    }
}
```

Lavas 2.0 依赖的 WorkBox 使用更灵活的方式，直接在 `/core/service-worker.js` 中编写代码，如：

```javascript
// Define runtime cache.
workboxSW.router.registerRoute(new RegExp('https://query\.yahooapis\.com/v1/public/yql'),
    workboxSW.strategies.networkFirst());
```

### 注册 Service Worker

Lavas 2.0 会自动注册 Service Worker，开发者无需进行额外开发或者配置。但如果您对此很感兴趣，可以参考 Lavas 文档中的[相关部分](/guide/v2/advanced/service-worker#注册-service-worker-扩展)
