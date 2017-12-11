# 9. 构建代码和上线

到了这里我们已经完成了一个基本应用的搭建，并赋予它 PWA 的两大特性。接下来我们把注意力放到代码的构建和上线上来。

在项目目录输入命令

```bash
lavas build
```

可以对 Lavas 项目进行构建，构建过程如下：

![lavas-build](http://boscdn.bpc.baidu.com/assets/lavas/codelab/lavas-build.png)

完成后，在项目根目录下会生成一个 `/dist` 目录，这边是构建后的项目代码。我们还可以通过命令

```bash
lavas start
```

来以线上模式运行项目。线上模式的所有代码均为压缩后的，并且 Service Worker 也会生效 (使用 localhost 访问即可)。

![lavas-start](http://boscdn.bpc.baidu.com/assets/lavas/codelab/lavas-start.png)
