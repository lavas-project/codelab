# 4. 添加简单登录页面

## 登录页功能

    - 提供一个简单的 “点击登录” 按钮，点击可以将 store 中的登录状态改为 true；
    - 提供一个前端跳转回到 Lavas 主页按钮，点击改按钮后，如果登录成功可以跳转至主页，否则还会重定向至登录页
    - 具体文件结构和代码如下

``` js

lavas-middleware
    └── pages
           └── login.vue

```


### （1）添加登录页 `./pages/login.vue`

``` html

<!-- login.vue -->
<template>
    <div>
        <router-link to="/"><h2>LAVAS</h2></router-link>
        <button @click="login()">点击登录</button>
        <div class="tips" ref="tips"></div>
    </div>
</template>

<script>

export default {
    name: 'login',
    methods: {
        async login() {

            // 如果未登录，设置登录成功
            if (!this.$store.state.common.login) {

                // 修改 store 中的登陆状态为 true
                await this.$store.dispatch('common/setLogin', true);
                this.$refs.tips.innerHTML = 'tips：登录成功';
            }
            // 如果登陆显示已登录
            else {
                this.$refs.tips.innerHTML = 'tips：已登录';
            }
        }
    }
};
</script>

<style lang="stylus" scoped>
h2
    margin-top 70%
    font-size 46px
    font-weight 500
    cursor pointer
button
    width 100px
    height 45px
    border solid 1px #ccc
    background #eee
    cursor pointer
.tips
    margin-top 10px
    color #666
    font-size 12px
</style>

```

### （2）访问 localhost:3000/login 即可看到添加的登录页啦！可以在首页增加到登录页的跳转，便于后续测试

![登录页](http://boscdn.bpc.baidu.com/assets/lavas/codelab/login-page.png)

![Lavas页](http://boscdn.bpc.baidu.com/assets/lavas/codelab/home-page.png)


> info
>
> 登录页添加完成
> 下一节我们根据需求添加具体的 store 文件，来管理我们的登录状态


