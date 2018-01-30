# 2. vue-awesome 的安装和注册

我们使用 [vue-awesome](https://github.com/Justineo/vue-awesome/) 提供 Icon 组件支持。

vue-awesome 内置了 Font Awesome 图标，可以方便地按需引入：

![Font Awesome](https://gss0.bdstatic.com/9rkZbzqaKgQUohGko9WTAnF6hhy/assets/pwa/projects/1515680651551/fontawesome-icons.png)

## 安装 vue-awesome

首先安装 vue-awesome：
```bash
npm i vue-awesome --save
```

## 注册 Icon 组件

我们希望在项目中以 `<icon></icon>` 方式使用 Icon，因此需要在 `core/app.js` 中完成组件的全局注册：
```javascript
// core/app.js

import Icon from 'vue-awesome/components/Icon.vue';

// 注册 Icon 组件
Vue.component('icon', Icon);
```

至此我们就完成了 vue-awesome 的安装和注册工作，在下一节中我们将展示如何编写一个 svg-loader。
