# 2. 准备环境

1. 安装/升级 Lavas 命令行工具

    ```bash
    npm install lavas -g
    ```

2. 在合适的目录新建项目并命名，例如本教程中的 "lavas-2-sample"

    ```bash
    lavas init
    ```

    ![lava init](https://boscdn.baidu.com/assets/lavas/codelab/lavas-init-3.png)

3. 进入目录并安装依赖

    ```bash
    npm install
    ```

4. 启动开发服务器，访问 localhost:3000 将看到初始页面。
    ```bash
    lavas dev
    ```

默认启动 express 作为开发服务器。如果您想使用 Koa 作为服务器进行开发和上线，可以参考 [使用 Koa 作为服务器](/codelab/koa/introduction)。

接下来我们会通过一些代码的修改来进一步加深印象，了解 Lavas 的主要功能。
