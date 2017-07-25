第二部：用户登录
========

> 本章的目的在于教授大家 Vuex 的用法，以及逐步用 Vue 接管全局。

> 一段时间过去，我用这种看起来很土的方式改造了一部分代码，现在我希望更进一步，正好最近几个需求都处在验收阶段，我有一些时间可以做想做的事。

那就改造用户系统吧！

之所以选择这个方向，是考虑到用户登录与用户信息管理是另一个全局功能，和具体业务无关，但又在程序入口，通过对它的改造，我们有机会去触动更基础的部分，对将来彻底转向 Vue 有很大帮助。

说干就干，我翻查老代码，发现现在的处理方式，是在页面初始化的时候，向服务器发起一个请求，验证用户身份。

1. 如果已经登录，获取用户的信息，比如名字，可以看到的导航菜单等
2. 如果没有登录，则跳到登录页

这个过程其实还有点复杂，复杂就复杂在需要根据用户身份更新导航菜单。不过也正和我意，总得从外围向内核渗透么不是。

首先，修改整个 index.html，把左侧导航列表改成 Vue 模板：

```html
<ul id="navbar-side-inner" class="nav side-nav">
  <li class="nav-item">
    <a href="#/dashboard"><i class="fa fa-dashboard"></i><span>首页</span></a>
  </li>
  <li class="sidebar-nav-item" :class="invisible ? '' : 'hidden'" :id="'parent-' + item.link + item.subId" class="nav-item" v-for="item in sidebar">
    <template v-if="item.sub">
      <a href="javascript:void(0);" data-parent="#navbar-side-inner" data-toggle="collapse" class="accordion-toggle" :data-target="'#' + item.sub-id" v-if="item.sub">
        <i class="fa" :class="'fa-' + item.icon" v-if="item.icon"></i>
        <span>{{item.title}}</span>
        <span class="caret" v-if="item.sub"></span>
      </a>
      <ul class="nav collapse" :id="item.subId">
        <li :id="item.link" :class="invisible ? '' : 'hidden'" v-for="item in sub">
          <a :href="item.link">
            <i class="fa" :class="'fa-' + item.icon" v-if="item.icon"></i>
            <span>{{item.title}}</span>
            <button type="button" class="eye-edit-button" v-if="editing"><i class="fa fa-eye"></i></button>
          </a>
        </li>
        {{/each}}
      </ul>
    </template>
    <a :href="item.link" v-else>
      <i class="fa" :class="'fa-' + item.icon" v-if="item.icon"></i>
      <span>{{item.title}}</span>
      <button type="button" class="eye-edit-button" v-if="editing" v-if="editing"><i class="fa fa-eye"></i></button>
    </a>
  </li>
</ul>
```

如果你仔细看上面这段的代码，你会发现，它最多支持二级菜单，并且一二级菜单的 HTML 结构是完全一致的，这里为接下来组件一章埋下了伏笔。不过这一章我们姑且放过它。

然后把右上角的用户身份部分也改成 Vue 模板：

```html
<li class="nav-item dropdown me">
  <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">
    <span class="username">{{username}}</span> <span class="caret"></span>
  </a>
  <div class="dropdown-menu" role="menu">
    <a href="#/my/profile/" class="dropdown-item"><i class="fa fa-user fa-fw"></i> 我的账户</a></li>
    <a href="#/my/finance/" class="dropdown-item"><i class="fa fa-yen fa-fw"></i> 财务管理</a>
    <a href="page/my/changepwd.html" class="popup" title="修改密码" data-confirm="保存" data-cancel="取消"><i class="fa fa-fw fa-lock"></i> 修改密码</a>
    <a href="#/my/settings"><i class="fa fa-cog fa-fw"></i> 设置</a>
    <div class="divider"></div>
    <a href="#/user/logout"><i class="fa fa-fw fa-sign-out"></i> 退出</a>
  </ul>
</li>
```

模板告一段落。无论使用何种框架，保持对与 DOM 的解耦是很重要的。做的好的话，这个时候就能提现出效果了，此时我们直接用一个大的 Vue 实例去控制整个后台 UI，与原先的框架进行双轨制管理。

```javascript
// 原先的框架
var me = new tp.model.DIYUser()
var profile = new tp.view.Me({
  el: '.me',
  model: me
});
var body = new tp.view.Body({
  el: 'body',
  model: me
});
var sidebarEditor = new tp.view.SidebarEditor({
  el: '#navbar-side',
  model: me
});
var ranger = new tp.component.DateRanger({
  el: '.date-range'
});
var search = new tp.view.Search({
  el: '.global-search'
});

// 新的框架
let app = new App({
  el: 'body'
});
```

因为用户有关的部分都准备交给 Vue 处理，所以上面的 `profile` 和 `sidebarEditor` 都要干掉。还好里面都是处理界面逻辑的代码，干掉它们没什么大碍，最后就变成：

```javascript
// 原先的框架
var body = new tp.view.Body({
  el: 'body',
  model: me
});
var ranger = new tp.component.DateRanger({
  el: '.date-range'
});
var search = new tp.view.Search({
  el: '.global-search'
});

// 新的框架
let app = new App({
  el: 'body',
  data: {
    sidebar: null,
    me: null
  }
});
```