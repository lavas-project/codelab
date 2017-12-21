# 2. 准备环境

1. 安装 Lavas 命令行工具并初始化 Lavas 项目。

    如果对这块有所疑问，可以参考 [开发第一个 Lavas 应用](/codelab/get-started/introduction)

2. 可以对项目作适当的修改，例如添加入口 (entry), 添加页面，修改渲染模式 (SPA/MPA/SSR) 等等。

3. 保证项目可运行。启动 Lavas 开发服务器可以正常看到页面。

    ```bash
    lavas dev
    ```

4. 如果您想在线上实际运行时使用 Service Worker (App Shell 的实现依赖于 Service Worker)，您还需要让自己的站点能够通过 HTTPS 访问，并且在通过 HTTP 访问时强制跳转，因为 Service Worker 只对 HTTPS 站点生效。线下运行 (localhost) 不受影响。

因为 App Shell 的开发方式随渲染模式(是否服务端渲染)和入口数量(是否单个入口)有所差异，因此我们将以这两个维度来分别叙述。如果您的项目采用服务端渲染，您可以继续下一步的学习；如果采用 SPA/MPA 模式，您可以直接跳转到[这里](/codelab/appshell/spa)。

