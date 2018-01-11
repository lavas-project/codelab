# 1. 概述

使用 `lavas init` 创建的模版项目中，在以下场景下都会以编程方式使用 Lavas：
* `server.dev.js` 开发环境下的 SPA／SSR 模式。
* `server.prod.js` 生产环境下的 SSR 模式。

您可以阅读[这篇文档](/guide/v2/advanced/core-api)进一步了解技术细节。

在本教程中，我们将以 Lavas Basic 模版项目为基础，使用 Lavas API 开发自定义的错误处理和 SSR 中间件。
