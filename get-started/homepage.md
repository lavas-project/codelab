# 3. 查看主页

根目录下的 `/pages` 文件夹用于存放路由组件，Lavas 会自动根据文件结构创建路由规则，生成方法可以查看[文档](/guide/v2/basic/init#Lavas-自动路由生成方法)。

在初始模板中，`pages` 中如下的文件结构将生成三条路由规则：`/appshell`, `/` 和 `/error`。之前我们访问的主页对应着 `Index.vue`。另外，可以发现单页面组件的名称遵循了 Pascal 命名规范，这是 Vue 的编码规范推荐的[形式之一](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended)。
```
pages
├── Error.vue
├── Index.vue
└── Appshell.vue
```

让我们查看一下 `Index.vue`，这是一个标准的 Vue 页面组件，熟悉 Vue 开发的同学一定对它的结构相当了解：
```html
<template>
// 省略模板代码
</template>
<script>
export default {
    name: 'index',
    metaInfo: {
        title: 'Home',
        titleTemplate: '%s - Lavas',
        meta: [
            {name: 'keywords', content: 'lavas PWA'},
            {name: 'description', content: '基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题'}
        ]
    }
};
</script>
<style lang="stylus" scoped>
// 省略样式规则
</style>
```

其中 Lavas 使用了 vue-meta 对 html 的信息进行定义。其中的 `metaInfo` 是一个可配置的 key，通过修改 `/core/app.js` 中的相关配置进行修改。而具体包含的配置项以及涵义也可以参见 [vue-meta github](https://github.com/declandewet/vue-meta) 。

了解了 Lavas 对于路由组件的处理规则，下面我们将创建一个新的页面。
