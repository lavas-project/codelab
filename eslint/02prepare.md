# 2. 安装 ESLint

创建 Lavas 模板项目后，我们需要安装以下依赖：
* eslint
* [eslint-plugin-vue](https://github.com/vuejs/eslint-plugin-vue)，官方插件，包含对 `.vue` 文件的检查规则
* eslint-loader 在开发期间每次保存时自动检查 `.vue` 文件
* [babel-eslint](https://github.com/babel/babel-eslint) 转译 ESLint 还不支持的特性

```bash
npm install --save-dev eslint eslint-loader babel-eslint eslint-plugin-vue
```

安装完毕后在项目根目录下创建 `.eslintrc` 文件：
```
{
    "extends": "plugin:vue/essential",
    "parserOptions": {
        "parser": "babel-eslint",
        "ecmaVersion": 2017,
        "sourceType": "module"
    }
}
```
