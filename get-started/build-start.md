# 10. 构建代码和上线

对于一个应用来说，经过前面的步骤之后基本功能已经开发完整。让我们把注意力移动到代码的构建和上线上面来。

在项目目录输入命令

```bash
lavas build
```

可以对 Lavas 项目进行构建，构建过程如下：

![lavas-build](./images/lavas-build.png)

完成后，在项目根目录下会生成一个 `/dist` 目录，这边是构建后的项目代码。

## SPA 模式

由于 lavas 导出的模板默认采用的是 SPA 模式，因此 dist 目录下生成的是静态文件。

我们可以使用 lavas 提供的命令来进行启动。

```bash
lavas static
```

![lavas-static](./images/lavas-static.png)

如果您想脱离 lavas 命令行进行启动 (例如在线上机器进行启动)，可以使用例如 nginx 这类代理服务器对静态文件进行代理服务。

特别地，如果在路由模式中使用了 `history` 模式，您可能会想要使用 [connect-history-api-fallback 中间件](https://github.com/bripkens/connect-history-api-fallback)。lavas 会帮您生成您的项目中支持的所有路由，位于 `dist/lavas/routes.json`，在使用中间件时可能会用到。详细的配置方式可以参考 vue-router 提供的[这篇文档](https://router.vuejs.org/zh-cn/essentials/history-mode.html)

## SSR 模式

您可以修改 lavas.config.js 中的 ssr 为 true 来启用 SSR 模式。SSR 模式下，dist 目录下生成的是可运行的 nodejs 项目。首先我们使用命令

```bash
npm install
```

来安装所有依赖，并使用 Lavas 内置命令

```bash
lavas start
```

以线上模式运行项目。线上模式的所有代码均经过压缩，并且 Service Worker 也会生效 (使用 localhost 访问即可)。

![lavas-start](./images/lavas-start.png)

如果您想脱离 lavas 命令行进行启动 (例如在线上机器进行启动)，您有四个步骤需要进行。

1. 修改 `/server.prod.js`（注意并非 `/dist` 目录下而是根目录下），添加一行环境变量的声明，如下：

    ```javascript
    process.env.NODE_ENV = 'production';
    ```

2. 修改 `/lavas.config.js`，添加 `node_modules` 的复制要求：

    ```javascript
    build: {
        ssrCopy: isDev ? [] : [
            {
                src: 'server.prod.js'
            },
            {
                src: 'package.json'
            },
            {
                src: 'node_modules'
            }
        ]
    }
    ```

3. 运行 `lavas build`

4. 进入 `dist` 目录，执行命令 `node server.prod.js` 即可。
