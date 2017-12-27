# 1. 概述

Lavas 是一套基于 Vue 的 PWA 解决方案，可以帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题。如果您对 PWA 和 Lavas 还有任何疑问，可以在[我们的官方站点](https://lavas.baidu.com/)上找到二者的教程和文档。

在前一版 Lavas 的基础上，我们倾听了来自开发者的反馈，并跟随业界最新的技术和规范，升级完成了 Lavas 2.0 版本。您可以安装或升级 Lavas 命令行工具，并跟随本教程完成一个简单却完整的示例站点，享受 PWA 带来的全新浏览体验。让我们快速开始吧！

## Lavas 2.0 的改进点

* 不再需要事先选择模板，降低上手难度
* 服务端渲染 (SSR) 和客户端渲染 (SPA) 可以快速切换
* 自动生成路由规则，避免重复配置
* 针对 SSR 提供了更加合理的 AppShell 支持，配合 SPA 下的 Skeleton 保证用户享受更加顺滑和健壮的浏览体验

## 通过本教程您可以学到

* 如何使用更新后的 Lavas 命令行工具创建 Lavas 2.0 项目
* 如何开发 Lavas 2.0 项目

## 学习本教程前您应该掌握

* HTML, CSS(less或stylus), JavaScript 等前端编程基础
* ES6/7 部分常用语法，如 Promise, import/export 等
* Vue, Vuex, Vue-router 等基本的开发知识
* Webpack, Node.js 等基本知识 （__推荐__）

## 在开发前您需要准备

* 一台可以正常联网的计算机并已安装较新版本的 Node.js (≥ 5.0) 和 npm (≥ 3.0)
* 一个方便调试并支持 Service Worker 的浏览器，推荐使用 Google Chrome
* 一个自己习惯的文本编辑器，如 Sublime Text, Web Storm 等等
