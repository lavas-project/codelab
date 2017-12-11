# 4. 创建新页面和链接

一个站点只有一个首页显然是不行的，让我们再创建一个页面。这是一个附带了__动态参数__的页面。

## 创建页面

之前提过，Lavas 2.0 的一大改进点是根据 `/pages` 文件目录结构自动生成路由规则。这里让我们创建一个文章详情页面，它的访问路由路径为 `/detail/[id]`  (`[id]` 表示动态参数，一般来说是数字，如 `/detail/1` 即访问 id=1 的文章)。

Notes: Lavas 对 `/pages` 文件夹进行了监听，发生变动会重新生成路由规则，所以在以下操作过程中请保持开发服务器处于运行状态，如果已经关闭，请重新开启。

首先我们需要在 `/pages` 下创建一个子目录 `detail`，并在其中创建一个 Vue 页面组件 `_id.vue`， 文件结构如下：
```
├── Error.vue
├── Index.vue
├── appshell
│   └── Main.vue
└── detail
    └── _id.vue
```

`_id.vue` 的内容如下：
```html
<template>
    <div class="detail-wrapper">
        <article class="detail-content">
            <header class="detail-title">
                Detail {{$route.params.id}}
            </header>
            <!-- link to another detail -->
            <p>
            Progressive Web Apps are user experiences that have the reach of the web, and are:
Reliable - Load instantly and never show the downasaur, even in uncertain network conditions.
Fast - Respond quickly to user interactions with silky smooth animations and no janky scrolling.
            </p>
            <!-- link to index -->
        </article>
    </div>
</template>

<script>
export default {
    name: 'detail-_id',
    head() {
        return {
            title: `Detail ${this.$route.params.id}`,
            titleTemplate: '%s - Lavas',
            meta: [
                {name: 'keywords', content: `detail ${this.$route.params.id}`},
                {name: 'description', content: `detail ${this.$route.params.id}`}
            ]
        };
    }
};
</script>

<style lang="stylus" scoped>
.detail-content
    font-size 16px
    line-height 30px
    margin-top 30px

    .detail-title
        margin-bottom 20px
        padding 10px 0
        font-size 36px
        font-weight bold

</style>
```

从文件名称来看，Lavas 2.0 约定以下划线开头的 vue 文件是包含动态参数的页面，除了首字母不用大写之外 <s>(当然第一个也不是字母)</s>，下划线后面的内容 (如例子中的 `id`) 即为动态参数的名称。

从文件内容来看，让我们先忽略其中的两条 link 相关的注释。除了普通的页面内容之外，我们注意到在 `<template>` 和 `<script>` 中分别出现了 `{{$route.params.id}}` 和 `this.route.params.id`，这便是 Lavas 2.0 解析了 URL 之后产出动态参数的获取方式。举例来说，当用户访问 `/detail/1`，则这两个变量的值都是 `1`。

## 创建链接

这里我们来尝试在两个页面之间添加一些链接。开发者应当已经很熟悉 html 的 `<a>` 标签或者 vue 的 `<router-link>`标签。 虽然 Lavas 也支持这两种标签，但我们__强烈建议__用户使用我们的自定义标签 `<lavas-link>`，它的用法和 `<router-link>` 基本相同。

让我们在刚刚创建的 `detail/_id.vue` 的两处注释部分分别添加两个链接，代码如下：

```html
<!-- link to another detail -->
<lavas-link :to="{
    name: 'detailId',
    params: {
        id: Number($route.params.id) + 1
    }
}">Detail {{Number($route.params.id) + 1}}</lavas-link>

<!-- link to index -->
<lavas-link to="/">Back to home</lavas-link>
```

这里展示了两种 `<lavas-link>` 的使用方法，差别只在 `to` 属性。

1. `to` 属性是一个对象，由 `name` (必填) 和 `params` (选填) 组成。例子中的 `detailId` 是 Lavas 自动生成的路由名称，由文件路径和驼峰规则拼装而成 (`detail` + `_id => Id`)。因为 `detailId` 页面包含动态参数，所以允许我们再传入一个 `params.id`。 这样如果当前 URL 为 `/detail/1`， 那么点击之后就跳往了 `/detail/2`，以此类推。

2. `to` 属性是一个字符串，这种情况更加简单，直接填入目标页面的地址，也适用于外部链接。

经过两个简单的步骤，我们的第二个页面也完成了！保存文件，使用 `npm run dev` 启动项目之后，通过浏览器访问 localhost:3000/detail/1 即可看到这个新增的详情页面。

![step 4](http://boscdn.bpc.baidu.com/assets/lavas/codelab/lavas-2.0-sample-step4.png)

在进入下一个步骤之前，想一想你可以在首页添加一个链接跳往详情页吗？自己动手试试吧！
