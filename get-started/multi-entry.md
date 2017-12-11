# 5. 创建第二个入口 (扩展)

入口 (entry) 可以理解为多个页面 (page) 的集合，一个入口内部可以共享一些配置，包括路由方面的 `baseUrl`, `mode`，以及整个页面的外部框架等等。

让我们列举几个比较常见的入口使用场景来深入了解：

* 一个站点既提供 PC 服务又提供移动服务且两者页面并不共享，那么就可以创建两个入口来分别管理。

* 一个站点的两个模块在渲染模式 (是否服务端渲染)， 路由规则 (hash 或者 history) 或者外观样式，配色等不尽相同，则可以将这两个模块处理为两个入口。例如电商网站的母婴品类和数码3C品类。

通过 `lavas init` 的项目初始状态只包含一个入口，因此这里我们将创建第二个入口。如果您的最终开发计划中并不包括多入口，那您也可以跳过这里直接进入下一个步骤。

刚才我们已经创建了一个详情页面。一般来说详情页面和首页的外观布局就不太一样，例如详情页会多一个头部，会有一个按钮快速回到首页。既然已经 <s>强行</s> 有了需求，那让我们尝试创建一个入口吧。

## 修改入口配置

修改入口并不需要更改页面，因此 `pages` 目录不需要改动。我们打开根目录下的 `lavas.config.js` 文件 (包含几乎所有的配置项), 找到 `entry` 这一项 (初始状态应该包含一个名称为 `main` 的入口)，增加一个名称为 `detail` 的配置量，如下：

```
entry: [
    {
        name: 'detail',
        ssr: true,
        mode: 'history',
        base: '/',
        routes: /^\/detail/,
        pageTransition: {
            type: 'fade',
            transitionClass: 'fade'
        }
    },
    {
        name: 'main',
        ssr: true,
        mode: 'history',
        base: '/',
        routes: /^.*$/,
        pageTransition: {
            type: 'fade',
            transitionClass: 'fade'
        }
    }
]
```

通过路由匹配规则 `/^\/detail/`，我们成功地将刚才新建的详情页划归到新的入口 `detail` 中。但是新的入口只有配置，还没有实际的内容。

## 新增入口内容

让我们把注意点移动到 entries 目录中。为了简单起见，我们将已有的 main 目录内的所有内容复制一份，并命名为 detail。接着我们修改 detail/app.js ，使其引用自己的路由：

```
// import {createRouter} from '@/.lavas/main/router';
import {createRouter} from '@/.lavas/detail/router';
```

最后我们修改 detail/App.vue，实现先前提到的头部。为了简单我们只实现一个带颜色的头部作为示意。

```
<template>
    <div id="app">
        <!-- 添加header -->
        <header class="detail-header">Detail Header</header>
        <!-- 添加结束 -->

    <!-- 具体内容省略 -->

    </div>
</template>
```

并在 `<style>` 的最后增加样式

```
.detail-header
    background #ccc
    height 16px
```

保存文件后用浏览器查看 `http://localhost:3000/detail/1`，可以看到一个灰色的头部出现。

![step 5](http://boscdn.bpc.baidu.com/assets/lavas/codelab/step5.png)

有两点需要补充说明一下：

1. 为详情页添加头部的方法还有很多，可以直接在 \_id.vue 上方编写 (无法复用)，也可以通过父路由实现 (受限于路由规则)。这里只是为了演示创建入口而举的例子，并非舍近求远。

2. 因为目前只有两个页面 (首页和详情页)，所以添加了入口之后也仅仅影响到一个详情页而已。但当实际项目中页面数量增多之后，就能逐渐体现入口方案的优越性。
