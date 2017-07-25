## 使用 Vuex 保存用户状态

还记得我前面对比 React 时说过的话么？React 生态虽然丰富，但缺少官方指定库，所以质量参差不齐。Vue 在这方面做的比较好，官方有几个推荐产品组成全家桶。

虽然现在的改造东一榔头西一棒槌，但放长远来看，这套系统一定会整体迁移到 Vue，并且实现组件化。所以，为了能在组件间共享状态，我决定从现在开始就使用 Vuex。

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

如果你想了解 Vuex，建议去看[官方文档](https://vuex.vuejs.org/zh-cn/intro.html)，非常短小精悍，不超过1个小时你就能看完。

上一章里，我为老框架建立了一个接班人，准备逐步把工作交给它。接下来，我就会用 Vuex 构建一个全局数据中心，方便将来在系统内部存取数据。这次为了扩展考虑，我使用了 Vuex 的[模块](https://vuex.vuejs.org/zh-cn/modules.html)概念，即把不同的数据放在不同的模块里，方便管理。另外，从现在起，我会肆无忌惮地使用新技术，使用 Vue 开发的功能会通过 Webpack 打包到 JS 里（不包含原先的内容），加载到页面中。

```javascript
// index.js
import Vue from 'vue';
import Vuex from 'vuex';
import { API } from 'config';
import user from './user/';
import mutations from './mutations';
import actions from './actions';

Vue.use(Vuex);

export default {
  state: {
    API: API,
    _token: '',
  },
  mutations,
  modules: {
    user
  }
};

// user.js
import mutations from './mutations';
export default {
  state: {
    id: 0,
    name: '',
    fullname: '',
    email: '',
    sidebar: null
  },
  mutations
}
```

然后回到入口 JS，在里面添加用户身份检查：

```javascript
import base from './store/';

let store = new Vuex.Store(base);
let app = new Vue({
  el: 'body',
  store
});

let headers = new Headers();
headers.append('Accept', 'application/json');
fetch(API + 'login', {
  mode: 'cors',
  headers: headers,
  credentials: 'include'
})
  .then( response => {
    return [response.status, response.json()];
  })
  .then( (status, json) => {
    if (status === 401) {
      router.push('/login');
      return;
    }
    store.commit(UserMutations.SET_USER_INFO, json.user);
  })
  .catch( err => {
    router.push('/error/500');
  });
```

这里我用了 Fetch API，从服务器请求用户身份。如果服务器返回 401 错误，就说明用户没有登录，然后就通过路由跳转到登录界面；否则的话，把用户信息记入 `store` 完成操作。