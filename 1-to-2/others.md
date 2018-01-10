# 6. 其他文件的迁移

之前列出的都是开发者最容易修改的部分。除开它们之外，开发者还可能对其他目录的迁移感兴趣，这里进行简单的罗列。

## build 目录

build 目录和 webpack 编译相关，列出了全部的配置项和编译逻辑。但因为开发者极少修改但文件数量又不少，因此 Lavas 2.0 去除了这个目录，webpack 相关的配置放到了 `/lavas.config.js` 的 `build` 对象，详见[文档](guide/v2/advanced/build-config)

## config 目录

Lavas 2.0 将所有配置融合到一个文件 `/lavas.config.js` 中，不再分散成多个文件。您可以查看 Lavas 教程的 “进阶教程” 部分，选择某一模块进行查看。

## src 目录

除了上述介绍过的 components, page, store 目录之外，这里还有两大部分。

### assets 目录

对应 Lavas 2.0 的 `/assets` 目录，用于存放 __需要进行 webpack 构建__ 的外部资源。

### 其他散落文件

`entry-client.js`, `entry-server.js`, `app.js`, `App.vue` 对应 Lavas 2.0 `/core` 目录下的四个同名文件。

`entry-skeleton.js` 不再存在于用户目录，转由 Lavas 内部处理。`router.js` 也由 Lavas 自动生成。

## static 目录

对应 Lavas 2.0 的 `/static` 目录，区别于 `/assets` 目录，这里用于存放 __不需要进行 webpack 构建__ 的外部资源，会直接复制到 `/dist/static` 目录中。

