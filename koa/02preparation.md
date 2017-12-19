# 2. 准备环境

在本节中，我们会确保 Node 版本符合要求，并安装 Koa 完成准备工作。

## Node 环境

由于 Koa 使用了 ES7 async/await 语法，很多基于 Koa 的中间件都是直接使用，并没有使用类似 Babel 进行转译。所以 Koa 本身对 Node 的版本是有要求的，必须高于 7.6.0。

如果安装了 [n](https://github.com/tj/n) 或者 [nvm](https://github.com/creationix/nvm) 这样的 Node 版本管理器，可以方便地安装并切换到 7.6.0 以上的版本：
```bash
// 使用 n 安装 7.6.0 版本
n 7.6.0

// 使用 nvm 切换 7.6.0 版本
nvm use 7.6.0
```

## Koa

安装 Koa：
```bash
npm install koa --save
```

至此环境已经准备完成了，下面我们将使用 Koa 作为开发服务器。
