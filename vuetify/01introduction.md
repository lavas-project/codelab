# 1. 概述

Lavas 使用 Webpack 作为构建工具，众所周知 Webpack 功能强大但是配置十分复杂。Lavas 隐藏了大部分配置细节，仅暴露一些常用的快捷配置项供开发者使用，详见 [Lavas Webpack 构建配置](/guide/v2/advanced/build-config)。而对于高级开发者，Lavas 也提供了灵活的扩展方法 [`extend`](guide/v2/advanced/build-config#extend)，最大限度满足自定义配置需求。

在本教程中，我们将以 Lavas AppShell 模版项目为基础，使用相对复杂的 Webpack 配置实现 Vuetify 组件的按需加载。
