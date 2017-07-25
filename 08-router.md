第三步：路由
========

改造路由本身其实并不困难。真正的难点在于，之前的路由，无论是基于 [Backbone.Router](http://backbonejs.org/#Router) 还是基于 [Page.js](https://visionmedia.github.io/page.js/)，都是侦听 URL 变化，然后调用回调函数来处理。而 Vue 官方插件 Vue-Router，则是直接实例化页面组件。

这样导致我们很难渐进式的迁移功能页，必须小心翼翼的把两个路由分开，比如 `/new/path/to/feature/` 交给新路由，其它的交给老路由，等将来彻底迁移重构完毕再把 `new` 去掉。

本身路由的写法直接看[官方文档](https://router.vuejs.org/zh-cn/essentials/getting-started.html)就好了，大约1个小时就能读完，这里就不在赘述。