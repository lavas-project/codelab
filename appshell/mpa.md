# 6. MPA 的 Skeleton

如果采用前端渲染模式，项目中又包含多个 entry，那么这就是一个 MPA 项目。每个入口都会有一个 Skeleton ，这里来介绍如何实现。

为了表述方便，我们假设：

* 项目包含两个入口，分别为 `user`, `main`

* `/lavas.config.js` 的 entry 已经配置完成。`/^\/user/` ( /user 开头的路由)导流到 `user`， 其余导流到 `main`

## 编写 Skeleton

和 SPA 完全相同的步骤，这里不再详述。只是我们要编写多个 Skeleton ，如 `/entries/main/Skeleton.vue` 和 `/entries/user/Skeleton.vue`。

和 SSR 不同的是，Skeleton 是将内容和样式都包含在内的，因此不同的 Skeleton 应该是不同的，而不像 SSR 那样统一使用空的内容。

## Service Worker 配置

和 SPA 相同，不需要额外的配置。尤其注意的是，__不要使用__ SSR 模式下才需要的 `appshellUrls` 配置项。

## Service Worker 模板

修改 `/core/service-worker.js`。在这里我们将注册两个入口 HTML 的使用方式，通过 `whitelist` 进行区分：

```javascript
// Define response for HTML request.
workboxSW.router.registerNavigationRoute('/main.html');
workboxSW.router.registerNavigationRoute('/user.html', {
    whitelist: [/^\/user/]
});
```

WorkBox 在注册路由规则时内部采用逆序优先的方式，即__越在后面注册的规则会越优先进行匹配__。利用这个特点，我们使用和入口相同的导流正则作为 `whitelist`，让 `'/user'` 开头的路由全部使用 `/user.html`，而剩余的路由则使用默认的 `/main.html` 作为响应。
