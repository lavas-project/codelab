# 7. 配置 Manifest

一般来说，PWA 包含三大主要功能：桌面快捷方式，Service Worker 和消息推送。本节我们主要讨论如何赋予站点能够“添加到桌面快捷方式”的能力。由于 Service Worker 的内容比较灵活和复杂，因此我们单独编写了一个关于 Service Worker 的 [Codelab](/codelab/service-worker/introduction)。

这个功能主要针对通过移动端访问站点的用户。通过添加到手机桌面的快捷方式，用户能很便捷地调用本地浏览器直接打开站点。和普通浏览器的快捷方式不同，桌面快捷方式在打开时还包含一个启动动画，并且不包含普通浏览器的显示框架(如头部地址栏，底部菜单栏等)，使用起来就和本地安装的 APP 非常接近！

要实现这个功能，本质是需要在构建后的根目录配置一个 `manifest.json`，给浏览器提供必要的信息，具体可以参见[这里](https://lavas.baidu.com/doc/engage-retain-users/add-to-home-screen/introduction)。

在 Lavas 中，我们可以通过把这个 `manifest.json` 放置在 `/static` 目录下。在构建之后，Lavas 会将它复制到正确位置，即 `/dist/manifest.json`。

让我们尝试编写一个示例的 `manifest.json`，看看它的基本功能：

```json
{
    "start_url": "/?utm_source=homescreen",
    "name": "*__name__*",
    "short_name": "*__name__*",
    "icons": [
        {
            "src": "static/img/icons/android-chrome-512x512.png",
            "type": "image/png",
            "size": "512x512"
        },
        {
            "src": "static/img/icons/android-chrome-192x192.png",
            "type": "image/png",
            "size": "192x192"
        },
        {
            "src": "static/img/icons/apple-touch-icon-180x180.png",
            "type": "image/png",
            "size": "180x180"
        },
        {
            "src": "static/img/icons/apple-touch-icon-152x152.png",
            "type": "image/png",
            "size": "152x152"
        }
    ],
    "display": "standalone",
    "background_color": "#000000",
    "theme_color": "#278fef"
}
```

配置完之后，我们可以在 Google Chrome 的开发者工具 (F12) 的 Application - Manifest 查看配置是否成功，正常配置的效果如下：

![chrome](https://boscdn.baidu.com/assets/lavas/codelab/step7.png)

如果条件允许，你也可以尝试在手机上直接查看效果。如果你没有一个可访问的 https 域名的话，下面列举一种代理配置方法。

## 如何在手机上查看本地站点的 PWA 特性

1. 电脑上安装代理软件 (charles, fiddler等等)

2. 以 fiddler 为例，打开 Tools - Telerik Fiddler Options - Connections 配置/记录代理端口 (Fiddler listens on port)， 如 `8888`

3. 打开 Tools - HOSTS, 添加一行 `127.0.0.1      localhost`，将 `localhost` 定向到电脑本地

4. 手机和电脑连接同一个 WIFI 网络，修改手机的 WIFI 配置，找到高级设置，填入电脑的 IP 和刚才记录的端口号 (例如 `192.168.1.2:8000`)

5. 打开手机中支持 PWA 特性的浏览器 (推荐 Google Chrome)，输入 URL `localohst:3000` (3000为 lavas 默认服务端口)，能够展现首页的话，在右上角菜单中也可以看到“添加到主屏幕”

    ![mobile-1](https://boscdn.baidu.com/assets/lavas/codelab/step7-mobile-1-2.png)

6. 添加完成后，在手机桌面上就可以看到新增的图标了

    ![mobile-2](https://boscdn.baidu.com/assets/lavas/codelab/step7-mobile-2-2.png)

7. 点击这个新的图标，在短暂的过场动画后，会直接展现页面，没有浏览器的显示框架，Cooooooooool!!!!

    ![mobile-3](https://boscdn.baidu.com/assets/lavas/codelab/step7-mobile-3-2.png)

这里有两个注意点：

1. 第四步中，查看自己电脑的 IP 可以在 fiddler 右上角的 'Online' 用鼠标悬浮看到，也可以在终端输入其他命令查看(如 Windows 的 `ipconfig -all`， Linux 的 `hostname -i` 等等)。一般在 WIFI 配置完成后，手机会自动发送一些请求探测网络是否可用，这些请求如果可以在 fiddler 上追踪到，则表明连接成功，HOSTS 也就可以生效了。
2. 部分手机会有权限控制，禁止应用创建桌面快捷方式。这种情况下，我们需要额外打开手机设置，找到权限管理，将 Google Chrome 的“创建桌面快捷方式”设置为允许。这也是 PWA 面临的一个支持问题，但相信随着 PWA 的普及和着实提升的用户体验，这类壁垒最终将会被打破。
