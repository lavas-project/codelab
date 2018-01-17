# 3. Vuetify 按需加载

Vuetify 提供了[组件按需加载](https://vuetifyjs.com/vuetify/a-la-carte)特性，虽然仍然处于测试版本。

## 安装 Babel 插件

按照官方文档，首先需要安装 `babel-plugin-transform-imports` Babel 插件，以特定的转换方式对 `import` 进行转译：
```bash
npm install --save-dev babel-plugin-transform-imports
```

## 修改 Babel 配置

然后我们需要扩展 Lavas 的 Webpack 构建配置，具体需要修改 [`babel`](/guide/v2/advanced/build-config#babel) 配置项：
```javascript
// lavas.config.js

build: {
    babel: {
        presets: ['vue-app'],
        plugins: [
            "transform-runtime",
            ["transform-imports",
                {
                    "vuetify": {
                        "transform": "vuetify/es5/components/${member}",
                        "preventFullImport": true
                    }
                }
            ]
        ],
        babelrc: false
    }
}
```

## SSR 模式下添加白名单

如果当前处于 SSR 模式下，我们还需要向添加 `nodeExternals` 添加[白名单](/guide/v2/advanced/build-config#nodeexternalswhitelist)，将 Vuetify 源文件打包到 `server-bundle` 中。SPA 模式可以忽略这一步：
```javascript
// lavas.config.js

build: {
    nodeExternalsWhitelist: [
        /vuetify/
    ]
}
```

## 引入 Stylus 源文件

在样式文件中引入 Stylus 源文件而非编译好的全集 CSS 文件：
```javascript
// assets/stylus/main.styl

// @import '~vuetify/dist/vuetify.min.css'
@import '~vuetify/src/stylus/app'
```

## 按需引入组件

最后修改引入 Vuetify 的代码，位于 `core/app.js` 中，我们只引用使用到的 Vuetify 组件，在 AppShell 模版项目中，我们只使用到了 `VApp`，`VBtn` 和 `VIcon` 这三个组件：
```javascript
// core/app.js

import {
    Vuetify,
    VApp,
    VBtn,
    VIcon
} from 'vuetify';

Vue.use(Vuetify, {
    components: {
        VApp,
        VBtn,
        VIcon
    }
});
```

## 最终效果

让我们验证下按需加载效果，同样打开 `bundleAnalyzerReport` 并使用 `lavas dev` 启动开发服务器：
```javascript
// lavas.config.js

build: {
    bundleAnalyzerReport: true
}
```

分析结果如下：
![vuetify-alacarte](https://boscdn.baidu.com/assets/lavas/codelab/vuetify-alacarte-2.png)

从分析图表中可以看出效果十分明显：
* `vuetify.js` 496.2KB -> 30.6KB
* `vuetify.min.css` 547.71KB -> 42.5KB
