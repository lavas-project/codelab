# 3. 扩展 Webpack

安装完毕后，我们需要为 Webpack 新增一条规则，让 eslint-loader 处理 `.js` 和 `.vue` 文件。
通过 [extendWithWebpackChain](https://lavas.baidu.com/guide/v2/advanced/build-config#extendwithwebpackchain) 方法，我们很容易进行以下扩展：
* 首先我们删除掉 Lavas 在开发模式中内置的 `no-emit-on-errors` 插件，这个插件会在 Webpack 编译出错时终止整个编译流程。而对于 ESLint 检测出的不符合规范的错误，我们希望允许 Webpack 继续编译，因此选择去掉这个插件。
* 然后我们配置针对 `.js` 和 `.vue` 文件的处理规则，使用 `eslint-loader` 处理。
```javascript
extendWithWebpackChain(config, {type, env}) {
    // eslint 检测出错误时，依然继续编译
    config.plugins.delete('no-emit-on-errors');

    // 处理 .js 和 .vue 文件
    config.module.rule('eslint')
        .test(/\.(js|vue)$/)
        .use('eslint')
            .loader('eslint-loader')
            .end()
        .exclude.add(/node_modules/);
}
```
