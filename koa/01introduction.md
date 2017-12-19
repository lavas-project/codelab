# 1. 概述


查看 Lavas 2.0 初始项目的服务端脚本可以发现，我们选择 express 作为服务器：
* 在 `server.dev.js` 中，SPA/MPA/SSR 开发模式下使用 express 作为开发服务器
* 在 `server.prod.js` 中，SSR 场景下使用 express 作为线上服务器

[Koa](https://github.com/koajs/koa) 作为下一代 Web 框架，Lavas 也提供了对此的支持。在本教程中，我们将演示如何使用 Koa 作为开发和线上服务器。

## 开始本教程前应该了解

* [Lavas](https://lavas.baidu.com/guide/v2/basic/introduction) 初始化教程
* [Koa](https://github.com/koajs/koa) 基础使用文档
