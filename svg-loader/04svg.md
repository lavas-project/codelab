# 4. 使用自定义 SVG 图标

之前我们使用了 vue-awesome 内置的 Font Awesome 图标。
在本节中，我们将使用第三方 SVG 图标。

之前我们在配置中定义了：
* 存放第三方 SVG 图标的文件夹，例如 `assets/svg`
* 前缀，便于通过 `svg-my-icon` 这样的名称引用

```javascript
// lavas.config.js

icon: {
    // 前缀
    prefix: 'svg-',

    // 用户自定义的 svg 文件夹
    svgDir: path.join(__dirname, 'assets/svg'),

    // 项目中使用的 fontawesome 名
    icons: [
        'envelope'
    ]
}
```

## 获取第三方 SVG

在 iconfont 上下载一个[购物车](http://www.iconfont.cn/collections/detail?spm=a313x.7781069.1998910419.de12df413&cid=31) SVG 图标，并放入 `assets/svg` 下，完整路径为 `assets/svg/cart.svg`。

## 使用第三方 SVG

在 `pages/Index.vue` 中使用，注意需要带上我们配置的前缀：
```html
<template>
    <div>
        <h2>LAVAS</h2>
        <h4>[ˈlɑ:vəz]</h4>
        <icon name="envelope"></icon>
        <icon name="svg-cart"></icon>
    </div>
</template>
```
