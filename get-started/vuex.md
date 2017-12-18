# 6. 使用 Vuex 管理数据

[Vuex](https://vuex.vuejs.org/zh-cn/) 是一个专为 Vue.js 应用程序开发的状态管理模式，一般用来管理页面的数据获取和访问。

接着上一节的话题，如果我们需要发送请求获取数据并且在页面中使用，一个更加常见且合理的方式是使用 Vuex 来管理，因此这一节我们会改造上一节那种“原始”的方式，让代码变的更具结构性和扩展性，也更符合常规线上应用的要求。

更多关于 Lavas 和 Vuex 交互部分的写法也可以参考[Lavas 文档](/v2/advanced/store)。

## 创建 store

首先我们在根目录下的 `/store` 目录内创建一个 `detail.js`，内容如下：

```javascript
import axios from 'axios';

const SET_WEATHER = 'setWeather';

export const state = () => ({
    weather: {
        text: '',
        temp: 0
    }
});

export const mutations = {
    [SET_WEATHER](state, {weatherText, weatherTemp}) {
        state.weather = {
            text: weatherText,
            temp: weatherTemp
        };
    }
};

export const actions = {
    async setWeather({commit}, params) {
        try {
            let url = `https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20%3D%20${params.woeid}&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys`;
            let result = await axios(url);

            let condition = result.data.query.results.channel.item.condition;

            commit(SET_WEATHER, {
                weatherText: condition.text,
                weatherTemp: condition.temp
            })
        }
        catch (e) {
            // TODO with error
            console.log('error in setWeather');
            console.log(e);
        }
    }
};
```

关于 Vuex 的组成 (state, mutations, actions) 以及写法等在这里不做介绍，在他们的[教程](https://vuex.vuejs.org/zh-cn/)上有详尽的介绍。

这里我们可以看到，原先在上一节中发送请求的部分移动到了 Vuex 的 action `setWeather` 中，因此对 `axios` 的引入也改到了这里。此外为了模拟现实情况，我们把城市ID (woeid) 当做参数由外部传入，而不是像上一节那样写死在代码中。

Lavas 会自动扫描 `/store` 目录中的文件，逐个以 Vuex 的模块 (module) 进行加载。因此不必再在别处进行配置即可直接使用了。

## 使用 store

我们需要修改 `/pages/detail/_id.vue`，让他可以使用我们刚才建立的 Vuex 模块来获取天气数据，代码如下：(还是只列出 script 部分)

```javascript
import {mapState} from 'vuex'

export default {
    name: 'detail-_id',
    head() {
        // 内容省略，和第四节相同
    },
    async asyncData({store, route}) {
        await store.dispatch('detail/setWeather', {woeid: 2151849});
    },
    computed: {
        ...mapState('detail', [
            'weather'
        ])
    },
    created() {
        console.log(`Weather of Shanghai: ${this.weather.text}, ${this.weather.temp}°F`);
    }
};
```

首先关注 `asyncData` 方法，这里不再直接发送请求，而是调用我们刚刚创建的 Vuex 模块的 `dispatch` 方法来告知 store 发送请求获取数据。

接着我们在这里使用 Vuex 的快捷函数 `mapState`，将状态 `weather` 挂载到 `this` 中。

最后我们采用了 Vue 的生命周期钩子 `created()` 打印获取到的天气信息，这也是为了和上一节的例子保持一致。真实的应用中，我们更可能将天气信息显示到页面上而非打印在 console 里。

至此我们已经完成了示例应用的基本搭建，并且虽然简单却包含了很多功能，包括多个页面，跳转链接，发送异步请求和使用 Vuex 管理数据。接下来我们要学习如何使用 Lavas 的核心功能，让站点拥有 PWA 特性。
