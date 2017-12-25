# 3. SSR 模式下单入口应用的 App Shell

由于通过 `lavas init` 命令初始化的项目是采用服务端渲染 (SSR) 模式的，因此我们先关注 SSR 模式下的 App Shell 。如果您想了解 SPA/MPA 模式，可以直接跳转到[这里](/codelab/service-worker/spa-mpa-config)。

我们先关注最简单的情况，即整个项目只有一个入口 (假设命名为 `main`，所有的路由规则都经过这个入口)。

![SSR App Shell](http://boscdn.bpc.baidu.com/assets/lavas/codelab/appshell.png)

## 编写 App Shell

首先我们需要为这个入口的所有页面开发一个 App Shell ([这是什么？](/guide/v2/advanced/appshell))。我们创建一个 `/pages/appshell/Main.vue`，并编写如下内容：

```
<template>
</template>

<script>
export default {
    name: 'appshell-main',
    metaInfo: {
        title: 'Lavas',
        meta: [
            {name: 'keywords', content: 'lavas PWA'},
            {name: 'description', content: '基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题'}
        ],
        bodyAttrs: {
            'empty-appshell': undefined
        }
    }
};
</script>
```

这一段最重要的是 `metaInfo` 中的 `bodyAttrs['empty-appshell']`，虽然值是 `undefined`，但因为 Lavas 的内部机制这段__不能删除__，否则将导致 App Shell 无法工作。其余部分都可以根据开发者项目的具体情况自行修改。一般来说 App Shell 的 `<template>` 留空就可以。([原因？](/guide/v2/advanced/appshell#Skeleton-和-App-Shell-的差异))

## Service Worker 配置

打开根目录下的 `/lavas.config.js`，找到 `serviceWorker` 段的 `appshellUrls` 配置项，做如下配置：

```javascript
module.exports = {
    // ...
    serviceWorker: {
        // other service worker config
        appshellUrls: ['/appshell/main']
    },
    // ...
};
```

和 App Shell 有关的配置项只有一个 `appshellUrls`。如果您对其他配置项感兴趣，可以参考 Lavas 文档的 [Service Worker 部分](/guide/v2/advanced/service-worker)。这里的 `appshellUrls` 填入的是 App Shell 本身 vue 组件的可访问路径。

## Service Worker 模板

Service Worker 模板位于 `/core/service-worker.js`，在这里我们将注册 App Shell 的使用方式，其余 Service Worker 模板的固定内容就不再重复了。

```javascript
// Define response for HTML request.
workboxSW.router.registerNavigationRoute('/appshell/main');
```

和 App Shell 相关的只有 `registerNavigationRoute` 这一句。这句的作用是将所有 HTML 请求 (`request.mode === 'navigate'`)由 Service Worker 进行拦截，使用参数中注册的 App Shell 进行返回。这里注册的 `'/appshell/main'` __必须__ 包含在 Service Worker 配置项 `appshellUrls` 数组中，否则将无法注册成功。

经过这些配置，我们的应用已经实现了 App Shell。在首次被访问时，会将 App Shell 缓存起来，并在后续访问时直接从缓存取出并返回，大大提升了响应速度，提供更顺滑的加载体验！
