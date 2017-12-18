# 5. store 中添加登录状态管理

## 需要支持的状态管理功能

- state 中增加 login 参数，供中间件及页面查看是否登录
- actions 提供可以改变 login 值的方法，便于模拟登录操作
- 具体的文件结构和代码如下

```

lavas-middleware
    └── store
           └── common.js

```


（1）添加 `./store/common.js` 文件，state 中添加了 login 参数，可以供前端跳转验证登陆状态，另外还有两个 actions:

- setLogin() 供前端模拟登录使用，登录页 “点击登录” 按钮，则将 login 改为 true；
- validLogin() 供服务端模拟登录验证使用，可向服务端验证登录情况，并设置 login 值，此处为了方便直接模拟未登录，直接将 login 设置为 false;

``` js

const SET_LOGIN = 'setLogin';

export const state = () => {
    return {
        login: false
    };
};

export const mutations = {
    [SET_LOGIN](state, login) {
        state.login = login;
    }
};

export const actions = {
    setLogin({commit}, login) {
        commit(SET_LOGIN, login);
    },
    // 该方法主要供服务端接口验证使用
    async validLogin({commit}) {

        // 可以给服务端发请求，验证用户的登陆状态，此处模拟未登录
        let login = await new Promise( resolve => {
            setTimeout(() => {
                resolve(false);
            }, 1000);
        });

        // 并设置 store 中的登陆状态
        commit(SET_LOGIN, login);
    }
};

```

> info
>
> store 文件添加完成
>
> 下一节我们将写好的中间件进行简单的配置，即可测试了


