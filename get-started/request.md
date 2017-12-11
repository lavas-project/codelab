# 5. 发送请求

在同构应用中，服务端需要发送 HTTP 请求，而客户端需要发送 XHR 异步请求，很多库帮助开发者抹平了这种差异，[axios](https://github.com/axios/axios) 就是其中之一。在本节中，我们将使用 axios 请求雅虎天气数据并展示在页面上。

首先安装 axios:
```bash
npm install axios --save
```

接着在 `/pages/detail/_id.vue` 中，引入 axios：
```javascript
import axios from 'axios';
```

请求数据可以发生在路由导航完成之前或者之后，两种方式的特点可以参考 vue-router [数据获取](https://router.vuejs.org/zh-cn/advanced/data-fetching.html)一节。Lavas 默认采用了在路由导航完成之前请求数据，在顶部使用进度条给予用户视觉反馈。

为了支持服务端渲染 (SSR) ，每个页面组件都应当能够在服务端和浏览器端正常渲染。 Lavas 给开发者暴露了 `async asyncData() ` 方法，开发者一般通过实现这个方法来完成一些渲染前的操作，例如发送请求、获取数据和简单计算等，并且这些操作是同时支持服务端和浏览器端的。在本节中，我们就在 `asyncData` 方法内实现发送请求获取数据。

修改后的 `detail/_id.vue` 内容大致如下(仅 script 部分)

```javascript
import axios from 'axios';

export default {
    name: 'detail-_id',
    head() {
        // 内容省略，和第四节相同
    },
    async asyncData() {
        let result = await axios(`https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20%3D%202151849&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys`);
        let condition = result.data.query.results.channel.item.condition;

        console.log(`Weather of Shanghai: ${this.weather.text}, ${this.weather.temp}°F`);
    }
};
```

当浏览器首次访问页面 (或者在当前页面刷新) 时，Lavas 进入服务端渲染流程，因此上海的天气会打印在后端的命令行中

![server](http://boscdn.bpc.baidu.com/assets/lavas/codelab/lavas-axios-server-2.png)

而当后续访问时(例如点击页面上的链接跳转到其他详情页)，由浏览器端的 js 负责页面的跳转和渲染等，因此上海的天气会打印在浏览器的 console 中

![client](http://boscdn.bpc.baidu.com/assets/lavas/codelab/lavas-axios-client-2.png)
