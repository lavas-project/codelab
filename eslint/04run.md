# 4. 执行检查

启动 Lavas 开发服务器：
```bash
lavas dev
```

将看到控制台或者浏览器页面中会出现以下错误：
> /Users/baidu/workspace/baidu/tem/eslint/pages/Appshell.vue
> 1:1  error  The template root requires exactly one element  vue/valid-template-root

在 `pages/Appshell.vue` 中，我们使用了空的 `<template>` 用于在 SSR 中渲染 Appshell 内容。而 ESLint 禁止[根 template 为空](https://github.com/vuejs/eslint-plugin-vue/blob/master/docs/rules/valid-template-root.md)。我们可以在 `Appshell.vue` 中暂时关闭这条检查规则。
```html
// pages/Appshell.vue

<!-- eslint-disable-next-line vue/valid-template-root -->
<template>
</template>
```

再次运行 ESLint 将不再提示这个错误。
