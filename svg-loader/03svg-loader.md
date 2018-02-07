# 3. 编写 svg-loader

在本节中，我们将展示如何编写一个简单的 Webpack Loader，实现 Font Awesome 图标的按需引入。

## 按需引入配置

为了实现 Font Awesome 图标的按需引入，我们需要告知 Loader 项目中真正使用的图标集合，我们把图标相关的配置也放在 `lavas.config.js` 中，与 `build` 等配置项平级，具体代码如下：
```javascript
// lavas.config.js

build: {//...},
icon: {
    // 前缀
    prefix: 'svg-',

    // 用户自定义的 svg 文件夹
    svgDir: path.join(__dirname, 'assets/svg'),

    // 项目中使用的 fontawesome 名
    icons: [
        'envelope'
    ]
}
```

在本示例中，我们只引用一个 `envelope` 图标。

## 编写 svg-loader

接下来我们需要编写 svg-loader 指导 Webpack 注入按需加载 Icon 的源代码。

在项目根目录下新建 `build` 文件夹，并在其下创建 `svg-loader.js` 文件，代码如下：
```javascript
// build/svg-loader.js

const fs = require('fs');
const path = require('path');
const lavasConfigPath = require.resolve('../lavas.config.js');

module.exports = function (source) {
    // 删除require缓存
    delete require.cache[lavasConfigPath];

    const lavasConfig = require(lavasConfigPath);
    const iconConfig = lavasConfig.icon;
    const svgDir = iconConfig.svgDir;
    const icons = iconConfig.icons;
    const prefix = iconConfig.prefix;
    const PATH_REG = / d="([^"]+)"/g;

    // 验证`assets/svg`文件夹
    try {
        if (!fs.statSync(svgDir).isDirectory()) {
            throw new Error(`Invalid directory of svg: ${svgDir}`);
        }
    }
    catch (err) {
        this.emitError(err);
        return source;
    }

    // 监听`assets/svg`文件夹变化
    this.addContextDependency(svgDir);

    // 监听`lavas.config.js`文件变化
    this.addDependency(lavasConfigPath);

    // 从`assets/svg`文件夹中取
    fs.readdirSync(svgDir).forEach(file => {
        let svg = fs.readFileSync(path.resolve(svgDir, file), 'utf8');
        let sizeMatch = svg.match(/ viewBox="0 0 (\d+) (\d+)"/);

        let dMatch;
        let paths = [];
        let svgName = prefix + path.basename(file, path.extname(file));

        if (!sizeMatch) {
            return;
        }

        // 匹配多个 <path d=""> 路径
        while (dMatch = PATH_REG.exec(svg)) {
            paths.push({
                d: dMatch[1]
            });
        }

        // 注册使用到的svg
        source += `Icon.register(
            {
                '${svgName}': {
                    width: ${parseInt(sizeMatch[1], 10)},
                    height: ${parseInt(sizeMatch[2], 10)},
                    paths: ${JSON.stringify(paths)}
                }
            });`;
    });

    return source;
};
```

更多原理性的解释可以移步[这篇文章](https://lavas.baidu.com/guide/v1/webpack/svg-loader)查看。

## 在构建中使用 svg-loader

接下来需要在构建中通过 Webpack 配置使用我们刚刚编写的 svg-loader。具体使用到了 [`extend`](https://lavas.baidu.com/guide/v2/advanced/build-config#extend) 和 [`nodeExternalsWhitelist`](https://lavas.baidu.com/guide/v2/advanced/build-config#nodeexternalswhitelist)(SSR 模式下) 这两个配置项：
```javascript
// lavas.config.js

build: {
    // 扩展
    extend: function(config, context) {
        if (context.type === 'base') {
            // vue-awesome 中使用了 import，需要使用 babel-loader 预处理
            config.module.rules[1].exclude =
                /node_modules(?![\\/]_vue-awesome@\d+\.\d+\.\d+@vue-awesome[\\/])/;

            // 加入规则，向 core/app.js 中注入代码
            config.module.rules.push({
                resource: path.join(__dirname, 'core/app.js'), // 应用loader的文件
                loader: 'svg-loader',
                enforce: 'pre' // 声明svg-loader最先执行
            });

            // 指导 Webpack 查找 svg-loader
            config.resolveLoader = {
                alias: {
                    'svg-loader': path.join(__dirname, 'build/svg-loader')
                }
            };
        }
    },
    // SSR 模式下需要将 vue-awesome 以源码形式引入，SPA 模式可忽略
    nodeExternalsWhitelist: [
        /vue-awesome/
    ]
}
```

更多原理性的解释可以移步[这篇文章](https://lavas.baidu.com/guide/v1/webpack/svg-loader)查看。

## 验证效果

在 `pages/Index.vue` 也就是主页中使用 `envelope` 图标：
```html
<template>
    <div>
        <h2>LAVAS</h2>
        <h4>[ˈlɑ:vəz]</h4>
        <icon name="envelope"></icon>
    </div>
</template>
```

将看到主页中正确显示了这个信封图标：
![最终效果](https://boscdn.baidu.com/assets/lavas/codelab/svg-loader-02.png)

在下一节中，我们将展示如何引入第三方 SVG 图标。
