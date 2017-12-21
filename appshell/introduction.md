# 1. 概述

一般来说，PWA 包含三大主要功能：桌面快捷方式，Service Worker 和消息推送。关于桌面快捷方式和 Service Worker 的基本知识我们已经在基础篇 Codelab 的 [配置 Manifest](/codelab/get-started/manifest) 和 [配置 Service Worker](/codelab/get-started/service-worker) 中进行过介绍了。这篇教程我们重点介绍一下 Lavas 的 App Shell 功能。它基于 Service Worker 实现，可以让应用拥有更加顺滑的加载体验，而不是统一的白屏。关于这部分也可以参考 Lavas 文档的 [App Shell 部分](/guide/v2/advanced/appshell)，获得更多更详细的信息。

![appshell](https://lavas.baidu.com/doc-assets/pwa-doc/architecture/images/appshell.png)

## 通过本教程您可以学到

* App Shell 和 Skeleton 的概念以及如何运作
* 如何使用 Lavas 便捷地开发 App Shell

## 学习本教程前您应该掌握

* Service Worker 的基本知识 [链接](https://lavas.baidu.com/doc/offline-and-cache-loading/service-worker/service-worker-introduction)
* 如何开发第一个 Lavas 应用 [链接](https://lavas.baidu.com/guide/v2/basic/introduction)

## 在开发前您需要准备

* 一台可以正常联网的计算机并已安装较新版本的 Node.js (≥ 5.0) 和 npm (≥ 3.0)
* 一个方便调试并支持 Service Worker 的浏览器，推荐使用 Google Chrome
* 一个自己习惯的文本编辑器，如 Sublime Text, Web Storm 等等
