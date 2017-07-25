## 全局登录状态检查

用户登录状态也是后台产品比较关心的内容。因为后台有很严格的权限控制，不同权限的人看到的内容是不同的，甚至连菜单都是不同的。用户登录状态都保存在服务器，前端是不知道的，所以，很可能用户登录已经超时了，但是前端框架仍然傻乎乎的向后台发起请求。这个时候，后台必须能从服务器端返回的信息里看到端倪，并做出正确的处理，比如跳转到登录页。

以前用 jQuery，我们可以用全局的响应函数 `.ajaxError()` 来处理。如今我们正在做去 jQuery 化，所以这个工作也一样移交给 Vue 一族来处理。

Vue 生态里有个远程资源组件，叫做 [Vue Resouce](https://github.com/pagekit/vue-resource)，可以用来取代 jQuery.ajax。于是我把它也加入项目中，并且利用它的中间件来做全局登录状态检查：

```javascript
Vue.use(VueResource);
Vue.http.interceptors.push( (request, next) => {
  // 所有的头
  request.headers.set('Accept', 'application/json');
  request.credentials = true;
  next( response => {
    if (response.status === 401 || (!response.ok && response.status === 0)) {
      router.push({
        name: 'Login',
        params: {
          fetch: true,
          redirectTo: location.hash.substr(1)
        }
      });
    }
  });
});
```

不过需要注意的是，只有通过 Vue-Resource 发起的请求才会被中间件过滤，所以其它由 jQuery 发起的请求仍然继续用以前的策略处理。