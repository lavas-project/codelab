# 3. 配置多个 Skeleton

## 切换到 SPA 模式

多个 Skeleton 特性只能在 [SPA 模式](https://lavas.baidu.com/guide/v2/advanced/build-config#ssr)下生效，所以需要确认：
```javascript
// lavas.config.js

build: {
    ssr: false
}
```

## 编写多个 Skeleton

在 `core/` 目录下创建两个 Skeleton 组件 `TestSkeleton.vue` 和 `TestSkeleton2.vue`，内容可以在默认 `Skeleton.vue` 基础上略做修改。

## 配置路由路径映射

为了在不同路由路径下展示不同 Skeleton，需要配置映射表：
```javascript
// lavas.config.js

skeleton: {
    routes: [
        {
            path: '/test',
            componentPath: 'core/TestSkeleton.vue'
        },
        {
            path: '/test2',
            componentPath: 'core/TestSkeleton2.vue'
        },
        {
            path: '*',
            componentPath: 'core/Skeleton.vue'
        }
    ]
}
```

## 查看效果

启动开发服务器 `lavas dev`，分别访问 `/test` 和 `/test2` 可以看到展示了不同的 Skeleton 内容。
