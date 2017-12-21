# 4. SSR 模式下多入口应用的 App Shell

如果您的应用存在多个入口，一般来说这些入口的样式布局都不太相同，因此常规来说也需要与入口数量等量的 App Shell 作为加载时的填充。那么如何注册多个 App Shell 并分别应用于不同的入口呢？

为了表述方便，我们假设：

* 项目包含两个入口，分别为 `user`, `main`

* `/lavas.config.js` 的 entry 已经配置完成。`/^\/user/` ( /user 开头的路由)导流到 `user`， 其余导流到 `main`

## 编写 App Shell

和单入口完全相同的步骤，这里不再详述。差别是我们需要为每个入口都建立一个 App Shell ，如 `/pages/appshell/Main.vue` 和 `/pages/appshell/User.vue`。

不同入口的布局样式差别应该已经体现在 `/entries/[entry]/App.vue` 中了，因此不需要在 App Shell 中体现出任何差别，他们的 `<template>` 内容依然都是空。(当然 `metaInfo` 是可以不同的)

## Service Worker 配置

打开根目录下的 `/lavas.config.js`，找到 `serviceWorker` 段的 `appshellUrls` 配置项，做如下配置：

```javascript
module.exports = {
    // ...
    serviceWorker: {
        // other service worker config
        appshellUrls: ['/appshell/main', '/appshell/user']
    },
    // ...
};
```

`appshellUrls` 数组需要将所有要使用到的 App Shell 的可访问路径填入。

## Service Worker 模板

修改 `/core/service-worker.js`。在这里我们将注册两个 App Shell 的使用方式，通过 `whitelist` 进行区分：

```javascript
// Define response for HTML request.
workboxSW.router.registerNavigationRoute('/appshell/main');
workboxSW.router.registerNavigationRoute('/appshell/user', {
    whitelist: [/^\/user/]
});
```

WorkBox 在注册路由规则时内部采用逆序优先的方式，即__越在后面注册的规则会越优先进行匹配__。利用这个特点，我们使用和入口相同的导流正则作为 `whitelist`，让 `'/user'` 开头的路由全部使用 `/appshell/user`，而剩余的路由则使用默认的 `/appshell/main` 作为 App Shell。
