# 4. 数据存储迁移

在 Lavas 1.0 版本中，数据存储 (vuex store) 位于 `/src/store` 目录下，大致结构为：

```
└── src
    └── store
        ├── modules
        │   └── app-shell.js
        ├── index.js
        └── mutation.js
```

Lavas 1.0 版本 store 的思路是将常量 (`mutation.js`) 归类到一起，其他所有模块 (modules) 分开并位于 `modules` 目录中。`index.js` 负责将模块返回出去，作为引用的入口。

store 在 Lavas 2.0 进行了较大的改进，主要包括：

* 将常量声明集中在一起不利于编写每个模块，因此还是应该将常量声明在每个模块的上方，去除单独的 `mutation.js`
* 一个模块独立为一个文件，不再支持多个模块合并在同一个文件中
* 返回所有模块的 `index.js` 因为功能单一且配置重复，已经去除，由框架直接处理
* `index.js`, `mutation.js` 都已去除，因此 `modules` 目录也不再需要，直接将模块文件写在 `store` 目录下即可

## 重构 store

### 分离模块

观察模块 `/src/store/module/app-shell.js` 的结构：

```javascript
// 引入常量，定义state，定义actions等内容省略
export default {
    // state, actions, mutations
    modules: {
        appHeader: {...},
        appSidebar: {...},
        appBottomNavigator: {...}
    }
}
```

底部通栏已经删除，因此 `appBottomNavigator` 对象不再需要。因此我们可以将这个文件一分为二，分别为 `appHeader.js` 和 `appSidebar.js`。

此外 `isPageSwitching` 不属于模块，1.0 中直接使用 `appShell` 进行使用，因此这里我们也可以创建一个 `common.js` 来存放这个状态。

### 保持 namespace

观察 `/src/App.vue` 中对于 store 的使用：

```javascript
methods: {
    ...mapActions('appShell', [
        'setPageSwitching'
    ]),
    ...mapActions('appShell/appSidebar', [
        'showSidebar',
        'hideSidebar'
    ])
}
```

为了保持 namespace 不变，我们将三个分离出来的文件放入 `appShell` 目录。特别的，关于 `isPageSwitching` 状态的 namespace 将变成 `appShell/common`。

### 引用模块

如上所述，1.0 的 `/src/store/index.js` 来返回所有模块已不再需要，这部分工作由 Lavas 自动完成。因此当我们在 2.0 的 `/core/store/appShell/` 中放入三个文件时，自动已经可以被引用到了，不需要开发者额外的操作。

## 迁移后的目录结构

```
└── store
    └── appShell
        ├── appHeader.js
        ├── appSidebar.js
        └── common.js
```
