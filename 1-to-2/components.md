# 2. 组件迁移

在 Lavas 1.0 版本中，组件 (components) 位于 `/src/components` 目录下，大致结构为：

```
└── src
    └── components
        ├── AppBottomNavigator.vue
        ├── AppHeader.vue
        ├── AppMask.vue
        ├── AppSideBar.vue
        ├── ProgressBar.vue
        └── Sidebar.vue
```

Lavas 1.0 版本的 AppShell 模板中，默认存在六个组件。而 Lavas 2.0 将组件目录的位置移动到了根目录下，即 `/components`。因此我们需要做的就是把这些组件移动到 2.0 的 `/components` 目录即可。

## 事件总线

部分组件中还使用到了事件总线进行事件传输。因此 `/src/event-bus.js` 也应当随之迁移。理论上这个文件可以放在任何位置，只要引用路径正确即可。我们推荐放置在 2.0 的 `/core` 目录下，即 `/core/event-bus.js`。

不要忘记修改组件内对它的引用路径：

```javascript
// import EventBus from '@/event-bus';
// 修改引用路径
import EventBus from '@/core/event-bus';
```

__注意：__ `@` 在 1.0 中指向 `/src/`, 在 2.0 中指向 `/`。

## 使用组件

Lavas 1.0 在 `/src/App.vue` 中定义了整站的显示框架，因此在这里会大规模使用组件。起相同作用的文件在 2.0 位于 `/core/App.vue`。因为别名 (alias:`@`) 的存在，引用路径和名称等均未发生变化，所以开发者简单地将两者进行融合即可。

## 迁移后的目录结构

```
├── components
│   ├── AppBottomNavigator.vue
│   ├── AppHeader.vue
│   ├── AppMask.vue
│   ├── AppSideBar.vue
│   ├── ProgressBar.vue
│   └── Sidebar.vue
└── core
    ├── App.vue
    └── event-bus.js
```

## 重构一下？(扩展)

事实上在 Lavas 开发 AppShell 模板 2.0 版本之前，也是从 1.0 迁移而来。当时我们考虑除了纯粹的迁移，__还应当进行适当的重构__，因此虽然简单地将六个组件复制过去当然也可以运行，但 Lavas 做了更多。

如果开发者使用过 Lavas 2.0 的 AppShell 模板，可以发现在 2.0 中原生仅包含五个组件，分别是：

* AppHeader.vue
* AppSideBar.vue
* ProgressBar.vue
* Sidebar.vue
* UpdateToast.vue

和 1.0 比较，增加了 UpdateToast.vue, 去掉了 AppBottomNavigator.vue 和 AppMask.vue。

1. UpdateToast.vue

    用来处理 Service Worker 更新的组件，和 AppShell 本身无关，因此 Basic 模板也有。1.0 集成在 `/src/sw-register.js` 中，2.0 将它独立成了一个组件。

2. AppBottomNavigator.vue

    底部通栏(可以切换首页/个人中心)，但根据 1.0 发布后开发者的反馈来看，这个组件使用者并不多，因此 2.0 不再默认提供。

3. AppMask.vue

    1.0 中用作侧边栏浮出时的灰色遮罩，在 2.0 中集成到 AppSideBar.vue 中了，因此删除。

