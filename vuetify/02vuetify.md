# 2. Lavas AppShell 模版项目

[Vuetify](https://vuetifyjs.com/) 是一个基于 MaterialDesign 的组件库。使用 `lavas init` 创建的 AppShell 模版项目已经默认引入了 Vuetify。
```javascript
// core/app.js

import Vuetify from 'vuetify';
Vue.use(Vuetify);
```

由于我们引入了 Vuetify 全集，在 Vue 组件中可以使用任意的 Vuetify 组件，例如在 `components/AppHeader.vue` 中，使用了 `v-btn` 按钮和 `v-icon` 图标组件：
```javascript
// components/AppHeader.vue

<v-btn
    icon
    v-if="showMenu"
    @click.native="handleClick('menu')">
    <v-icon color="white" class="app-header-icon">menu</v-icon>
</v-btn>
```

## 引入全集的问题

这样引入全集的方式虽然使用起来方便，但会带来一个问题，就是会引入项目未使用的组件，增加了构建产物的大小。
为此，我们可以通过 [`bundleAnalyzerReport`](/guide/v2/advanced/build-config#bundleanalyzerreport) 配置项来查看当前的构建产物依赖情况。在 `lavas.config.js` 中打开这一选项：
```javascript
// lavas.config.js

build: {
    bundleAnalyzerReport: true
}
```

随后使用 `lavas dev` 启动开发服务器，在控制台中会发现启动了一个新的服务器，默认端口为8888：
```javascript
Webpack Bundle Analyzer is started at http://127.0.0.1:8888
```

分析图表如下：
![vuetify-alacarte](https://boscdn.baidu.com/assets/lavas/codelab/vuetify-alacarte-1.png)

可以看出 Vuetify 在构建产物中占比十分巨大，其中：
* `vuetify.js` 496.2KB
* `vuetify.min.css` 547.71KB

在下一节，我们将使用 Vuetify 的按需加载功能减少打包大小，同时进行 Webpack 自定义配置。
