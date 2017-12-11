# 8. 编写 Service Worker

本节将介绍 PWA 的另一大特性 Service Worker，这也是 PWA 三个特性中最能发挥开发者想象力和最复杂的部分。有关 Service Worker 的介绍可以移步 Lavas 官网的[相关章节](https://lavas.baidu.com/doc/offline-and-cache-loading/service-worker/service-worker-introduction)，这里不作过多描述。

大体来说，Service Worker 主要完成三个工作：

* __静态文件预缓存__ 能够提前预知的用户需要缓存的内容，通常是静态文件，例如 js, css, 字体文件等等。

* __动态缓存__ 用户在运行过程中实际发送请求后再进行缓存的内容，通常是动态的接口，因为含有动态参数所以不可能全部预缓存。动态缓存通常还有各类策略，如 networkFirst, cacheFirst 等等

* __appshell__ (或者 Workbox 称为 __navigationRoute__) __只在 SSR 下必要__ 首次打开站点时，额外请求一个路由将每个页面的框架 (或称 shell) 缓存住。之后每次请求页面，只要缓存中有这个框架，都直接返回并配上浏览器端渲染页面，用以提升渲染速度。

和上一节提到的 `manifest.json` 一样，开发者也可以自己编写一个 `service-worker.js` 放在根目录下并自行注册。但 Lavas 依然为开发者提供了便捷方式，让我们来尝试一下。

打开根目录下的 `lavas.config.js`，关注 `serviceWorker` 这一段，如下：

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
        appshellUrls: ['/appshell/main'],
        dontCacheBustUrlsMatching: /\.\w{8}\./
    },
    // ...
};
```
这些基本都是提供给 Lavas 内置的 WorkboxWebpackPlugin 使用的配置项。[Workbox](https://github.com/GoogleChrome/workbox) 是 Google 推出的 sw-toolbox 和 sw-precache 的升级版，封装了一些常用的 API (如预缓存，动态缓存及常用策略等)，帮助开发者更简单快速地开发 Service Worker。而 WorkboxWebpackPlugin 则是 Workbox 的 webpack 插件，通过配置项和模板两部分来生成 service-worker.js。我们先来看一下配置项部分。

## Service Worker 配置项

我们来看一下例子中使用的配置项(这些配置项基本都是必选的)。其余的可以参考 Workbox 的[官网](https://developers.google.com/web/tools/workbox/)

* __swSrc__ 生成 service-worker.js 所需的模板文件所在位置，后续会详细提及

* __swDest__ 生成的 service-worker.js 的存放位置。例子中放在了整体构建目录 (`/dist`) 的下面，即 `/dist/service-worker.js`

* __globDirectory__ 指定需要预缓存的静态文件的目录。例子设定为整体构建目录 (`/dist`)

* __globPatterns__ 相对于 globDirectory 指定的目录，指出哪些文件__需要__被预缓存。这里可以使用通配符，可以参考[node-glob#glob-primer](https://github.com/isaacs/node-glob#glob-primer)。例子中把 Lavas 构建生成的项目的静态资源全都囊括了。

* __globIgnores__ 相对于 globDirectory 指定的目录，指出哪些文件__不需要__被预缓存。和 globPatterns 一样，也可以使用通配符。例子中把 map 文件 (用于方便查看混淆后的代码) 和 sw-register.js (用于注册 sw，如果预缓存住则后续无法更新sw) 排除在外。此外 service-worker.js 本身也会被自动排除。

* __appshellUrls__ appshell 相关内容，后续会详细提及

* __dontCacheBustUrlsMatching__ Workbox 会将符合上述 glob 开头的三个配置项条件的所有静态文件逐个生成一个版本号 (称为 revision) 存入缓存，后续在面对同名文件时比较缓存中的版本号决定是否更新。但 Lavas 生成的静态资源文件绝大部分是在文件名中带有 hash 的 (如 `/dist/static/css/manifest.5e1ead3.js`)，一旦文件内容更新 hash 也会更新，因此 Workbox 内置的版本号就不再需要了，所以可以省略这个生成和比较的过程从而提升构建速度。因为 Lavas 生成的 hash 是8位的，所以例子中的正则也正匹配8位字母数字。

## Service Worker 模板

通过上述配置项，我们已经把静态文件的预缓存信息提供完整了，但是除了预缓存，service-worker.js 还有动态缓存和 appshell 这两个功能。因为这两个功能相对复杂，所以还需要 Service Worker 模板的支持。这一节我们来学习动态缓存应该如何编写，appshell 相关将留在下一部分。

Service Worker 的模板位于 `/config/service-worker.js` 在开始动态缓存之前，我们先要准备一些模板的基本内容，保证正常运行，如下：

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
```

第一行的 `importScripts` 是引用 Workbox 的API，这里的版本号还不支持自动修改，需要手动根据版本填入。不过 Workbox 的开发小组已经意识到这个问题，后续会修改 WorkboxWebpackPlugin 来修复。所以我们在实际开发项目中，最好把依赖的 Workbox 版本锁定，否则一旦更新，这里将会因为找不到对应文件而报错。

第二段创建 WorkboxSW 实例的代码中，我们可以指定我们应用的缓存 ID，这会最终影响到缓存的名称，我们要避免重名。

第三段的 `workboxSW.precache([]);` 看似是一句空语句，其实是一个代码插入点，刚才提过的预缓存的文件会经过 WorkboxWebpackPlugin 自动插入到这里，如果你感兴趣的话可以尝试看看构建后的 `/dist/service-worker.js`。

在这些准备工作之后，我们就将正式开始编写动态缓存。在之前的小节中，我们访问了雅虎天气的接口获取上海的天气。因此我们尝试将它缓存起来。

```javascript
// Define runtime cache.
workboxSW.router.registerRoute(new RegExp('https://query\.yahooapis\.com/v1/public/yql'),
    workboxSW.strategies.networkFirst());
```

Workbox 提供的 `resigerRoute` 方法接受两个参数，第一个是匹配请求 URL 的正则表达式，第二个是内置的缓存策略。除了例子中的 networkFirst，Workbox 还提供了 networkOnly, cacheFirst, cacheOnly, staleWhileRevalidate等等。关于这个方法的详细情况请参见 [API](https://developers.google.com/web/tools/workbox/reference-docs/latest/module-workbox-sw.Router#registerRoute)

经过这样的配置，每次请求雅虎天气并返回数据时会将数据进行缓存。如果网络连接故障，则返回缓存内容。配合预缓存了所有静态文件，站点就拥有了离线访问能力！

## appshell 和服务端渲染 (SSR)

经过上面两个步骤的配置，一个基本的 Service Worker 已经可以支持站点运行。但在服务端渲染 (SSR) 的情况下，有一种更优秀合理的 appshell 组织方式，这里进行拓展介绍。

关于组织方式本身，可以参阅 Lavas 专区在知乎上的 [SSR 架构项目实现离线可用（思路&案例）](https://zhuanlan.zhihu.com/p/30791448)。简单来说，我们把 appshell 的内容单独做成一个路由规则 (`/appshell/main`)。在用户访问任何页面时，额外发送请求获取并缓存 appshell。之后的每次页面请求 (navigation request)，只要缓存中存在 appshell，便返回立即展示并同时在浏览器端渲染剩余中心部分。也就是说，只有当首次访问 SSR 站点时，才会进入服务端渲染，此后的每次访问页面 (包括刷新页面)，只要缓存存在，都走浏览器端渲染流程，保证了离线可用和访问速度。

要实现这一点，需要 Service Worker 和服务端的多项支持，幸好 Lavas 已经帮我们封装了起来。我们需要做的只有三步：

1. 创建单独的 appshell 路由。Lavas 已经初始化在 `/pages/appshell/main.vue` 中

2. `/config/service-worker.js` 中添加 appshell 的声明，即本节第一部分配置项中的 `appshellUrls: ['/appshell/main']`

3. `/core/service-worker.js` 中添加注册，Workbox 内置了方法，可以直接使用。

    ```javascript
    workboxSW.router.registerNavigationRoute('/appshell/main');
    ```

因为示例的应用比较简单且 appshell 其实也没有什么特别的样式，因此肉眼观察可能看不出差别。我们可以通过 Chrome 的开发者工具观察 Network，看看第一个页面请求的返回数据即可发现，实际上这里并没有真正请求页面，而是被 Service Worker 拦截并返回了空的 shell，从 Application 的 Cache Storage 也能发现这一点。
