# 9. 构建代码和上线

对于一个应用来说，经过前面的步骤之后基本功能已经开发完整。让我们把注意力移动到代码的构建和上线上面来。

在项目目录输入命令

```bash
lavas build
```

可以对 Lavas 项目进行构建，构建过程如下：

![lavas-build](https://boscdn.baidu.com/assets/lavas/codelab/lavas-build-2.png)

完成后，在项目根目录下会生成一个 `/dist` 目录，这边是构建后的项目代码。我们还可以通过命令

```bash
lavas start
```

来以线上模式运行项目。线上模式的所有代码均为压缩后的，并且 Service Worker 也会生效 (使用 localhost 访问即可)。

![lavas-start](https://boscdn.baidu.com/assets/lavas/codelab/lavas-start-3.png)
