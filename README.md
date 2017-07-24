# 前言

Vue 横空出世，以迅雷不及掩耳之势横扫前端界，俨然有当年 jQuery 之势。我认为 Vue 成功的关键在于三点：

1. 学习曲线平缓，有点经验的前端基本上一天就能看完文档，然后就可以上手操作
2. 上升空间很大，组件化/路由/Vuex/Ajax，生态完整，架构强壮，用它构建中大型项目也很容易
3. API 设计优雅，并且和标准很友好

但是在我看来，很多 Vue UI 组件库反倒走在一条错误的道路上：过分追大求全。比如说，第一个组件多半是 Grid，CSS 能搞定的事情为什么要做成组件？前端本来就是 HTML/CSS/JS 的集合，我们理应把合适的技术用在合适的地方。

所以在这本书中，我会介绍这样的内容：

1. 一个基于 jQuery + Bootstrap 的后台项目
2. 对其进行有限的改造，让它能渐进地获得提升
3. CSS 的归 CSS，JS 的归 JS

## 适合的读者

1. 初中级前端，希望学习 Vue 和组件式开发
2. 后端，用过 Bootstrap，想升级改造框架

## 名词及约定

* ES6 = ES2015

* **MVVM** Model-View-ViewModel，一种现代化的 UI 架构体系，非常适合 HTML + CSS 这样的标记型语言，经实践证明可以大大提升开发效率。本文中大部分和 Vue 互相指代。
* **响应式** 在 CSS 领域，我们可以简单理解成手机端和桌面端有不同的呈现；在 MVVM 框架（本文上，它指界面根据数据自动刷新显示。

Vue 实现响应式的基础是 ES5 中的 `Object.defineProperty()` 方法，它把普通的属性读写改成 getter/setter，以便注入其它操作。大部分现代化浏览器都已经支持这个方法，但一些古老的浏览器如 IE8 不支持（现在还在使用 IE8 的人你也很难指望他们会升级），如果你要使用 Vue，请确保你的项目不会跑在这些平台上。

本文使用 Bootstrap 4.0.0-alpha.6，Vue 2.4.0。

## 作者介绍

大家好，我叫翟路佳，花名“肉山”，这个名字跟 Dota 没关系，从高中起伴随我到现在。

我热爱编程，喜欢学习，喜欢分享，从业十余年，投入的比较多，学习积累到的也比较多，对前端方方面面都有所了解，希望能与大家分享。

我兴趣爱好比较广泛，尤其喜欢旅游，欢迎大家相互交流。

你可以在这里找到我：

* [博客](http://blog.meathill.com)
* [微博](http://weibo.com/meathill)
* [GitHub](https://github.com/meathill)

## 版权许可

本书采用[“保持署名—非商用”创意共享4.0许可证](https://creativecommons.org/licenses/by-nc/4.0/)。

只要保持原作者署名和非商用，您可以自由地阅读、分享、修改本书。

## 反馈

如果您对于文中的内容有任何疑问，请在评论或 [Issue](https://github.com/meathill/ebook-vuerify-bootstrap/issues) 中告诉我。亦可发邮件给我：meathill[at]gmail.com。谢谢。
