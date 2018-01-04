# 3. 页面迁移

完成了组件迁移后，我们下一步迁移同样是 Vue 组件的页面。

在 Lavas 1.0 版本中，页面位于 `/src/pages/` 目录下，大致结构为：

```
└── src
    └── pages
        ├── Detail.vue
        ├── Home.vue
        ├── NotFound.vue
        ├── Search.vue
        ├── Skeleton.vue
        └── User.vue
```

2.0 中，页面直接放在 `/pages/` 目录下，因此和组件类似，也需要将这些 Vue 复制过去。但因为涉及到路由规则，所以还需要进行修改。

## 修改页面

1.0 对于页面组件的命名没有过多的限制，因为开发者可以在 `/src/router.js` 中进行 URL 和组件的映射配置。但 2.0 引入了 __自动生成路由__ 的功能，因此对名称也有了一定的规范，不过这个配置文件也不再需要了。

### Home.vue

2.0 要求首页命名为 `Index.vue`，因此我们需要将 `Home.vue` 进行重命名。

### Detail.vue

从 `/src/router.js` 配置中可以看出，Detail.vue 是一个接受动态参数的路由页面。2.0 可以直接从命名上实现动态参数，因此我们将 `Detail.vue` 放到 2.0 的 `/pages/detail/_id.vue`。

### NotFound.vue

Lavas 2.0 默认会生成 `Error.vue` 作为错误页面。虽然这个名称可以通过 `/lavas.config.js` 的 `errorHandler` 配置，但默认情况使用 `Error.vue` 可以节省一个配置项，这取决于开发者。

此外，2.0 还支持传入错误信息供错误页面显示使用，开发者可以通过 `this.$route.params.error` 获取。

### Skeleton.vue

因为骨架屏在构建后没有独立的 URL，但 2.0 会对 `/pages` 目录自动生成路由，所以骨架屏组件在 2.0 中放在 `/core/Skeleton.vue` 中。

### Search.vue, User.vue

Search.vue 是个普通页面(区别于首页和动态参数)，直接放到 `/pages` 目录下即可。而 2.0 去掉了底部通栏，所以用户中心页面也一并去掉了，`User.vue` 也就不存在了。

## 框架模板迁移

框架模板严格来说不属于页面，但因为也和显示相关，所以这里一并说明。

Lavas 1.0 中位于 `/index.html`，在 2.0 中位于 `/core/index.html.tmpl`。2.0 中为了统一 SPA 和 SSR，这个文件已经升级为模板，同时也提供了一些快捷方法。就这个示例项目来说，我们应该把模板编写成：

```html
<html>
    <head>
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <%= renderMeta() %>
        <%= renderManifest() %>
    </head>
    <body>
        <%= renderEntry() %>
    </body>
</html>
```

这也是 Lavas 2.0 的 `lavas init` 初始化项目的默认模板，适用于绝大部分情况。如果开发者确实有修改模板的需求，详细的信息可以参考[这里](/guide/v2/advanced/core#indexhtmltmpl)。

## 迁移后的目录结构

```
├── pages
│   ├── detail
│   │   └── _id.vue
│   ├── Error.vue
│   ├── Index.vue
│   └── Search.vue
└── core
    └── index.html.tmpl
```
