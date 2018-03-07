# 4. 创建新页面和链接

一个站点只有一个首页显然是不行的，让我们再创建一个页面。这是一个附带了__动态参数__的页面。动态参数的概念和 Lavas 的处理可以参考[这篇文档](/guide/v2/basic/init#Lavas-自动路由生成方法)。

## 创建页面

Lavas 对 `/pages` 文件夹进行了监听，发生变动会重新生成路由规则，所以在以下操作过程中请保持开发服务器处于运行状态，如果已经关闭，请重新开启。

首先我们需要在 `/pages` 下创建一个子目录 `detail`，并在其中创建一个 Vue 页面组件 `_id.vue`， 文件结构如下：
```
├── Error.vue
├── Index.vue
├── Appshell.vue
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
    metaInfo() {
        return {
            title: `Lavas Sample Detail ${this.$route.params.id}`,
            titleTemplate: '%s - Lavas',
            meta: [
                {name: 'keywords', content: `Lavas Sample Detail`},
                {name: 'description', content: `Lavas Sample Detail ${this.$route.params.id}`}
            ]
        }
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

让我们先忽略其中的两条 link 相关的注释。除了普通的页面内容之外，我们注意到在 `<template>` 和 `<script>` 中分别出现了 `{{$route.params.id}}` 和 `this.route.params.id`，这便是 Lavas 2.0 解析了 URL 之后产出动态参数的获取方式。举例来说，当用户访问 `/detail/1`，则这两个变量的值都是 `1`。

## 创建链接

这里我们来尝试在两个页面之间添加一些链接。熟悉 Vue 的开发者应该对 `<router-link>` 标签并不陌生。

让我们在刚刚创建的 `detail/_id.vue` 的两处注释部分分别添加两个链接，代码如下：

```html
<!-- link to another detail -->
<router-link :to="{
    name: 'detailId',
    params: {
        id: Number($route.params.id) + 1
    }
}">Detail {{Number($route.params.id) + 1}}</router-link>

<!-- link to index -->
<router-link to="/">Back to home</router-link>
```

这里展示了两种 `<router-link>` 的使用方法，这在 [router-link 官方文档](https://router.vuejs.org/zh-cn/api/router-link.html) 中有详细描述。

这里需要指明的是，因为和普通 Vue 项目不同，Lavas 项目的路由规则是自动生成的。因此当使用 `to` 的 `name` 属性时，需要将路径转化为 __驼峰形式__，这样才能和 Lavas 生成的路由规则一致。例如例子中的 `detailId` 是由文件路径和驼峰规则拼装而成 (`detail` + `_id => detailId`)的。

经过两个简单的步骤，我们的第二个页面也完成了！保存文件，使用 `npm run dev` 启动项目之后，通过浏览器访问 localhost:3000/detail/1 即可看到这个新增的详情页面。

![step 4](https://boscdn.baidu.com/assets/lavas/codelab/lavas-2.0-sample-step4.png)

在进入下一个步骤之前，想一想你可以在首页添加一个链接跳往详情页吗？自己动手试试吧！
