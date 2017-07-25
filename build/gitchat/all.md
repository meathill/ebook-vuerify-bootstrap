# 前言

Vue 横空出世，以迅雷不及掩耳之势横扫前端界，俨然有当年 jQuery 之势。我认为 Vue 成功的关键在于三点：

1. 学习曲线平缓，有点经验的前端基本上一天就能看完文档，然后就可以上手操作
2. 上升空间很大，组件化/路由/Vuex/Ajax，生态完整，架构强壮，用它构建中大型项目也很容易
3. API 设计优雅，并且和标准很友好

但是在我看来，很多 Vue UI 组件库反倒走在一条错误的道路上：过分追大求全。比如说，第一个组件多半是 Grid，CSS 能搞定的事情为什么要做成组件？前端本来就是 HTML/CSS/JS 的集合，我们理应把合适的技术用在合适的地方。

当然，如果你是想把一切都塞进 JS 的“JS 至上党”，这样做也行。不过，不是每个人都要从头开始做一个新产品，很多同学正在维护一个老产品，如何把新技术应用到老产品里，让它老树开新花，也是本文的主旨。

所以接下来，我会介绍这些内容：

1. 前端框架很多，为什么要使用 Vue？
2. 一个基于 jQuery + Bootstrap 的后台项目
3. 对其进行有限的改造，让它能渐进地获得提升
4. CSS 的归 CSS，JS 的归 JS

## 适合的读者

1. 初中级前端，希望学习 Vue 和组件式开发
2. 后端，用过 Bootstrap，想升级改造框架

## 名词及约定

* ES6 = ES2015

* **MVVM** Model-View-ViewModel，一种现代化的 UI 架构体系，非常适合 HTML + CSS 这样的标记型语言，经实践证明可以大大提升开发效率。本文中大部分和 Vue 互相指代。
* **响应式** 在 CSS 领域，我们可以简单理解成手机端和桌面端有不同的呈现；在 MVVM 框架（本文上，它指界面根据数据自动刷新显示。

Vue 实现响应式的基础是 ES5 中的 `Object.defineProperty()` 方法，它把普通的属性读写改成 getter/setter，以便注入其它操作。大部分现代化浏览器都已经支持这个方法，但一些古老的浏览器如 IE8 不支持（现在还在使用 IE8 的人你也很难指望他们会升级），如果你要使用 Vue，请确保你的项目不会跑在这些平台上。

本文使用 Bootstrap 4.0.0-alpha.6，Vue 2.4.0+。

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

# 前端框架发展简史

对前端框架不感兴趣的同学可以跳过这一章，并不会影响后面的理解。

这章主要解决这些问题：

1. 为什么要切换到 Vue 上？
2. jQuery 真的过时了么？
3. 除了 Vue 以外，我还有其它选择么？

--------

我之前还做过一次视频分享 [jQuery, Backbone, Vue](https://segmentfault.com/l/1500000008694676)，也是通过分析 JS 开发的历史，对比三个时期最具代表性的框架。感兴趣的同学可以看一下。
史前文明
========

有位名人说过：“一个人的命运啊，当然要靠自我奋斗，但是也要考虑到历史的行程。”这句话放在 Web 技术上，其实也非常正确。

发明 HTML 的目的是为了方便阅读文献，所以它身上有很多印刷业的影子。后来大家嫌排版没有其它工具做得漂亮，于是又发明了 CSS。结果凭借着极高的传播效率，市面上又缺少竞品，急速普及，被大家拿来干各种事情。

在那个拨号上网的洪荒年代，浏览器还非常初级，与服务器进行数据交互的唯一方式就是提交表单。用户填写完成之后，交给服务器处理，如果内容合规当然好，如果不合规就麻烦了，必须打回来重填。所以很容易想象：当用户填完100+选项，按下提交按钮，等待几十秒甚至几分钟之后，反馈回来的信息却是：“您的用户名不能包含大写字母”，他会有多么崩溃多么想杀人。为了提升用户体验，网景公司的[布兰登·艾克](https://zh.wikipedia.org/wiki/布蘭登·艾克)用了大约10天时间，开发出 JavaScript 的原型，从此，这门注定改变世界的语言就诞生了。
# 石器时代 - jQuery

这个时候浏览器还处于非常严重的分裂状态。IE 自恃有 Windows 护身，真是想怎么搞怎么搞。另一边的网景后来的 Firefox 空有一腔报国热血，但是怎么都干不过 IE。后来干脆全捐给开源社区，曲线救国，用免费开源的群众战争和微软打。

这种环境受苦的还是网页开发者。同一个页面，经常要写两套代码，然后寻找各式各样的 Hack 方法，费时费力。于是 jQuery 出现后大家觉得真是“春风十里，不如你”。

jQuery 的口号是：“Write less, do more“，为 Web 开发做出了卓越的贡献：

1. 提供统一的操作接口。你不需要担心代码跑在什么浏览器上，自有 jQuery 来适配。
2. 封装了很多常用接口。比如增删样式，只需要 `.addClass(className)` 就可以，比原始方法简单很多。
3. 用组合模式减少错误。HTML 结构多变，JS 操作 DOM 节点很容易发生错误，尤其早期切页面和写 JS 的很可能不是同一个人。用 jQuery 你至少不用担心会报错。

就这样，jQuery 凭借着出色的工程设计，俘获了大量开发者的心，是现在最普及的框架。
铁器时代 - Backbone
=======

jQuery 治下的 DOM 操作实在太简单了，选择器 + 修改，还不用担心报错。这就好比说，邻居有个热心大哥，买了辆车，说你出门尽管找他。然后他还真的来者不拒，笑脸相迎，以至于小区里的人出门都不开车也不打车也不骑车，都坐他的车。

开始总是好的，但随着环境变化，老的优势也可能会变成问题。就像一个小区只有一辆车肯定不够一样，开发者对 DOM 操作过分依赖，会导致 JS 代码和 HTML 严重耦合，牵一发而动全身，维护成本大幅升高。

另外，经过几年发展，包括 Flash 等软件不懈探索，此时的 Web 已经不仅仅要提供能阅读的图文信息，越来越多的 RIA(Rich Internet Application，富互联网应用) 涌现出来，大家都在竭尽所能地把桌面软件搬上互联网。这对前端开发提出了更高的要求。

Backbone 给出了自己的答案。它是一个 MVP 框架，充分利用了现代 Web 技术，包括 jQuery。它大大减少了操作 DOM 的需求，转而教大家使用模板。它以数据为视角来组织代码，告诉大家：原来 JS 还可以这么写。

![Backbone Model View](./img/intro-model-view.svg)

使用 Backbone 编写的应用在工程性方面提升巨大，维护成本大大降低，后期增删改功能都变得相对容易。开发中大型软件也变得可行，开发者感受到技术架构带来的价值，更加主动的放眼看世界，果然还有更值得我们学习的东西。
蒸汽动力 - Knockout，Angular1
========

MVC 是 Smalltalk 提出的编程模型，实际上，那个时期的概念跟今天相距甚远：主要输入设备是键盘，鼠标指针在屏幕上移动都需要开发者自己写，更没有“元素-点击事件”这种极其抽象的东西。所以它里面的 Model-View-Controller 不能用现在的概念去套。(感兴趣的同学可以去看下扩展阅读里头两篇文章。)

随着 UI 技术的发展，接下来出现的是 MVP 模型。它里面的 P（Presenter）已经是 UI 控件了，所以和 Web 技术非常接近。Backbone 就是这样架构的框架。

接下来，UI 技术进一步发展，HTML 这种标记语言如日中天，于是微软最早提出了 MVVM 的概念，并且将它应用在自家产品中。它的模式是这样的：

![MVVM 图示](./img/mvvm.png)
(图片来源：https://erazerbrecht.wordpress.com/2015/10/13/mvvm-entityframework/)

View 视图和 ViewModel 双向数据绑定，View 既是数据展示窗口，也是接受用户操作的输入来源。ViewModel 除了负责渲染逻辑以外，还负责全局数据管理，以及和真正的数据源 Model 进行交互。MVVM 非常吸引人，因为它在 MVP 的基础上又进行了一次抽象，并且提供双向数据绑定，所以开发维护效率巨高，代码量可能只有 MVP 的 1/10，但功能一致甚至更强。

最早在 Web 中引入这套模式的是 Knockout。用英语的句式来说：它是如此之早以至于它甚至支持 IE 6 和 Firefox 3.5……于是这也让它背上了沉重的历史包袱：

1. 它必须显式的声明数据绑定，显式到语法啰哩吧嗦
2. 赋值时必须使用 `.get(key)` `.set(key, val)` 这种语法，甚至 `a.get('key1').get('key2').get('key3').set('key4', val)` 这样

另一个尝试来自 Angular1。它不需要这么复杂的语法，而是采用“脏查询”的方式，即当某个可能导致页面重新渲染的操作产生后，检查所有备案过的变量，如果有改变的，就重新渲染。这种做法的问题就是慢，在运算能力充足的桌面电脑上感觉不明显，但是在移动端就会很慢。桌面端也会慢，所以它只能追踪1000个变量。

但是它的开发效率的确很高，在企业级占据霸主地位，几乎已经是事实标准。

这两个框架有些生不逢时，因为浏览器整体环境的限制，它们选择了不够完善的实现方案。但是 Angular 至少证明了这个方向可行，而且效果很好，于是他们注定要被后浪拍在沙滩上。
电子时代 - Vue，React，Angular2/4/5
========

接下来便轮到我们的主角登场了。不过与它一起登场的还有另外两名选手，都是背景深厚实力不凡的大 Boss，我先介绍它们。

## React

React 是 Facebook 研发的框架。它最大的特点是使用虚拟 DOM 作为实际 DOM 的影子，这样当它判断是否需要重新渲染时，就不用费力的与 DOM 交互，而只要从 JS 内存中取出虚拟 DOM 做 diff 即可。这样做可以大大提升检查效率，提高渲染速度。

但它本质上仍然在执行脏检查，所以实际效率不太高，或者说，有明显的天花板；只是面对越来越强的计算能力，这些损失浪费的表现越来越不明显。但是虚拟 DOM 也带来另外一个好处：如果重写它的实现机制，可以在任意场合实现任意类型的渲染，包括移动 App。于是便诞生了 React Native 这个项目，可以直接把基于 React 开发的 Web 项目转译成原生应用。

React 背靠 Facebook 这座大山，国内也有阿里支持，社区异常活跃，生态异常丰富。又能编译出原生应用，这是它最大的优势。

不过 React 也有缺点，导致我没有选择它：

1. 要使用 React 必须使用丑陋的 JSX，我认为这是反标准的
2. 它的生态虽然丰富，但是官方并没有引导，没有全家桶，各种实现参差不齐
3. 设计上为照顾大规模企业级开发，里面有众多新概念，学习曲线非常陡峭
4. 夹带私货，如今 Apache 基金会已经禁止使用 React

## Angular 2/4/5

Angular 吸取了第一代的教训，在 Angular 2 里重写了底层逻辑，也使用虚拟 DOM 来判断是否需要重新渲染。所以 React 的优势它基本也有。之后的 v4、v5都是在 v2 基础上的改进，并没有特别大的变化。

Angular 的学习曲线就更陡峭了，应该说它从一开始就没打算走群众路线，而是直接奔着大而全的企业级开发框架去做的（因为 v1 就是这样做的并且取得了成功）——相对来说 React 表现的还有点扭捏。

另外 Angular 为了能够更好的对接大规模团队开发，选择 TypeScript 作为开发语言，一方面使得学习成本更高；另一方面也和标准化渐行渐远。这也是我不推荐它的原因。

## Vue

与前面两个竞品不同，Vue 是个人作品，而且在 2013 年才开始开发。但我选择它推荐它并不是因为什么挑战大公司霸权的情怀，而是它的确设计得很棒。

### 1. 双向绑定效率高

问世的晚，历史包袱就少。Vue 使用 ES5 新增的 `Object.defineProperty()` 方法，将对象的属性转化为 `getter/setter`，这样我们习以为常的 `this.a=1` 赋值语句实际上就被改写成 `this.set('a', 1)`，而这个操作对开发者来说是完全无感的！这样我们一方面可以正常写代码，另一方面还可以轻松的享受到双向绑定，并且是高效的没有多余动作的绑定，任何有点点洁癖或者强迫症的人都会觉得很舒服吧！

### 2. 模块化减缓学习曲线

初入门时，我们只需要学习 Vue 就好。如果只想实现简单的“数据<=>视图”映射，区区几行代码就足够了，非常简单。甚至可以直接拿来替换 jQuery。

之后随着项目增大，使用加深，可以慢慢的开始使用组件，使用路由，使用全局状态管理。这一系列进化都在平缓的进行。

### 3. 贴近标准

与大公司喜欢夹带私货，搞自有标准不同，Vue 是标准友好的。甚至连 Vue 控件，写出来都跟普通 HTML 一样。作为个人开发者，或者小公司开发人员，我不愿意介入大公司之间的角逐，我见过妖魔横行的年代，我希望标准一统天下。

--------

## 小结

在代表先进生产力的 MVVM 框架里，我最终选了 Vue 作为新的主攻框架。我也把它推荐给大家。
框架入门
========

这一章会简单介绍 Bootstrap 和 Vue 的入门知识，如果您已经了解，可以跳到下一章。
Bootstrap 简介
========

Bootstrap 是 Twitter 的两位前端工程师搞出来的前端框架。它包含了大量 UI 组件，可以覆盖到很多开发场景，大大提升开发效率。

Bootstrap 经过几轮升级，现在处于 v4-alpha.6 阶段，作为一个免费开源产品，原作者太忙，alpha 快两年了，还没 beta，不知道什么时候能正式版。不过经过我一段时间的使用，我觉得目前这个版本基本上没啥大问题，还是推荐大家使用。

[Bootstrap 4 alpha 6 文档](http://v4-alpha.getbootstrap.com)

作为最流行的前端框架，有很多人基于它做出了很多值得学习的项目，很多都是开源免费的，也一并推荐给大家：

* [Bootstrap 免费主题](https://bootswatch.com/)
* [Bootstrap 付费模板](https://wrapbootstrap.com/)
* [CoreUI](http://coreui.io/) 基于 Bootstrap 做的后台类模板，免费，有多种框架配置
* [Start Bootstrap](https://startbootstrap.com/) 另一套免费模板

Bootstrap 的文档非常详细，我就不凑字数了。将来文章发布后若有问题再补充吧。
Vue 入门
========

Vue 的文档非常棒，尤其还有中文版，学习起来轻松愉快。

[Vue 官方教程中文版](https://cn.vuejs.org/v2/guide/)

这里我也不再重复官方已有的内容了，大家自己看就好。说一下我理解中，从 jQuery 向 Vue 转换时需要注意的东西。

## 1. MVVM 对数据的抽象

jQuery 里几乎没有数据抽象。你面对的就是一个虽然错综复杂，但是总能找到联系的 DOM 树，只要你有耐心，总能把它改成你想要的样子。

MVP 就抽象出数据层和视图层，但是还要我们手动更新视图；MVVM 比 MVP 的抽象更进一步，只要操作数据。所以我们必须要理解它的抽象，并且习惯它的抽象。

在这个体系里，我们应避免直接操作 DOM，因为一切都是数据的映射。举个例子，一个新闻列表，在传统开发模式中，是无数个 DOM 操作的结果；而在 Vue 里，就是通过模板把数据映射成 HTML。对后台类产品而言，这很好理解也很好实现，因为后台可以抽象成用户与数据的交互，然后还原成数据的展示和修改，继而直接对应到屏幕上的组件上。

在写 Vue 应用的时候，我们需要注意，哪些数据是业务数据，即要拿来跟后端数据进行交互的；哪些数据是界面数据，即用来切换页面状态，和业务无关。但是基本上，我们不需要直接操作 DOM。

## 2. 组件复用

组件是 Vue 最值得注意的强大特性。组件化和组件复用将大大提升我们的开发效率。

使用组件主要有两种方式：

1. 注册全局组件。这种方式很简单，有点类似 jQuery 插件，我们只要引用组件就好，然后就可以在模板中使用特定的组件标签。比较适合已有项目，可以在不怎么改动的前提下接入应用 Vue。
2. 使用局部组件。这种方式要复杂一些，而且也有几种不同的实现，如果同时要加载组件模板和组件样式，可能还要用 webpack + vue-loader。

因为这篇文章就是“渐进式改造项目”，所以根据项目现状选择合适的方法很有必要。

## 3. ES6 与生态

这个其实不是 jQuery 和 Vue 的差别，只是在眼下这个时间点，ES6 已经实装到绝大部分浏览器里，所以我们无论是看文档、看教程都会看到大量 ES6 的内容。至于整个前端生态，基于 Node.js 开发的各种工具也已经普及到方方面面，使用 webpack + 各种 loader 已经成了默认功课。

所以，那个用 `<script src="/path/to/jquery.js">` 引入 jQuery，然后就可以在页面当中任意使用 jQuery 相关技术的年代，其实已经过去了。
目标改造项目简介
========

这个项目，本来有机会成为一个真实的项目，但造化弄人，所以它现在只是一个虚拟的项目。

![截图](./img/snap.jpg)

这个项目是这样的：

1. 一个后台产品，主要面向公司内部员工，提供对公司产品的维护
2. 后台已经工作了5年，现在仍在正常工作，不能下线
3. 后台不断有新需求，不太可能另起炉灶开发
4. 后台使用 Bootstrap + jQuery 搭建，是一个前后端分离结构，每个子页面加载完成后，会有一个组件管理器对子页面进行扫描，初始化组件，并且完成数据加载
5. 公司的角度，对 Vue 即不支持也不反对，只要求必须保证开发效率和工作效率
6. 虽然这个产品很重要，但是公司不会投入更多的人力，从头到尾都只有原先的几个人来做

现在，作为一个有追求有梦想，并且想在跳槽的时候有更多加分的前端开发者，我要对它进行 Vuerify 改造了！
第0步：分析
========

好的，啰嗦半天半天终于进入实战了。不过，在动手之前我还要多说两句。所谓谋定而后动，想清楚再动手，肯定比头脑一热上去猛干要好。

一个项目能跑5年，说明其设计大体上还是过硬的。所以一开始最好不要动其根本，先找比较独立的一块儿来弄最为合适。

我们看一眼 Bootstrap 的[文档](https://v4-alpha.getbootstrap.com/getting-started/introduction/)，首先 [布局（Layout）](https://v4-alpha.getbootstrap.com/layout/overview/) 和 [内容（Content）](https://v4-alpha.getbootstrap.com/content/reboot/) 的部分就不需要组件化了，CSS 搞得定的事情就不劳 JS 动手了。

[组件（Component）](https://v4-alpha.getbootstrap.com/components/alerts/)里面，有很多也是纯 CSS（比如Badge） 或者可以进行纯 CSS 改造的（比如 Navs），这些我们先不管，即使改造也等将来用 CSS 改造或者直接放到大组件里。还有像 [Tooltip](https://v4-alpha.getbootstrap.com/components/tooltips/) 这样，虽然带着一点 JS，但实际上对 JS 的需求很低，我们也不用管它，只要 jQuery 一日不死，这些旧臣都能尽忠报国。

看来看去，最值得拿来入手的应该是 [弹窗（Modal）](https://v4-alpha.getbootstrap.com/components/modal/) 和 [通知（Alert）](https://v4-alpha.getbootstrap.com/components/alerts/) 之类和主要业务逻辑关系不大，自身可定制性比较强（JS 比较复杂）的组件。

好的，就拿弹窗来试刀吧！

--------

篇幅所限，我没法展示对每一个组件的重构/改造，所以本文中的几个篇章都有特定的目的，主要在于介绍一门或几个技术。希望大家举一反三，将它们应用到日常工作中。
第一步：弹窗 Modal
========

> 本章的目的在于领大家入门。

弹窗除了用作通知以外，在后台产品当中还经常用来对特定属性进行修改，如下图所示：

![后台](./img/modal-snap.jpg)

对于这样的需求，在之前的产品中我是这么做的，首先，HTML 部分就按照 Bootstrap 的标准套路来写：

```html
<div class="modal fade" id="sales-editor">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">修改职位</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <form action="/api/sales/" data-action="/api/sales/{{id}}" method="POST" id="sales-editor-form">
          <input type="hidden" name="user_id" value="">
          <div class="form-group">
            <label for="type">职位</label>
            <select class="form-control" name="type" id="type">
              <option value="1">区域总监</option>
              <option value="2">商务经理</option>
              <option value="3">商务助理</option>
            </select>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-primary" form="sales-editor-form"><i class="fa fa-check"></i> 保存</button>
        <button type="button" class="btn btn-secondary" data-dismiss="modal"><i class="fa fa-times"></i> 取消</button>
      </div>
    </div>
  </div>
</div>
```

表格模板是这样的（只写单行吧，其它部分意义不大）：

```handlebars
<tr>
  <td>{{name}}</td>
  <td><button type="button" class="btn btn-primary edit-button" data-id="{{id}}">编辑</button></td>
</tr>
```

这两部分没什么好解释的。接下来 JS 的部分就稍显啰嗦：

```javascript
// 通过后端接口取出来所有商务数据
let allSales = fetchAllSales();

// 编辑按钮的点击事件，每次点击，除了打开新窗口，还要把表单里面的几个值修改一下
$('#table').on('click', '.edit-button', event => {
  let id = $(this).data('id');
  let sales = allSales[id];

  $('#sales-editor').modal('show')
    .find('[name=user_id]').val(id)
    .end().find('form').attr(function () {
      return $(this).data('action').replace('{{id}}', id);
    })
    .end().find('select').val(sales.type);
});
```

很明显，正如前文所写的那样，jQuery 用起来很方便，可以在 DOM 节点之间自由穿梭。但是因为它没有抽象，所以任何操作都要手工编写，会造成 JS 和 HTML 深度耦合，不仅不方便维护，新功能开发的时候也要写很多多余的代码。

接下来我们要用 Vue 实现同样地功能，应该怎么写呢？

首先我们可以观察一下 Bootstrap 里 Modal 的实现机制，很明显，它是通过修改 `.modal` 元素的 `display` 属性和增删 `show` 样式来实现的。那很简单，我们知道，Vue 里，可以通过 `:prop="someValue"` 把 vm 的值 `someValue` 绑定到 DOM 节点的 `prop` 属性上，所以控制弹窗的打开关闭就很简单：

```html
<div class="modal fade" :class="isShow ? 'show' : ''" :style="isShow ? 'display:block' : ''" id="sales-editor" @click.self="close">
  <!-- 内容 -->
</div>
<div class="modal-backdrop fade" :class="isShow ? 'show' : ''" v-show="isShow"></div>
```

```javascript
let app = new Vue({
  el: '#app',
  data: {
    isShow: false
  },
  methods: {
    show() {
      this.isShow = true;
    },
    close() {
      this.isShow = false;
    }
  }
});
```
（这里只列出重点代码，其它因为不影响大局就没有写。具体可以参考 [codepen oegbvK](https://codepen.io/meathill/pen/oegbvK?editors=1010)）

这样修改了之后，我发现打开关闭已经正常工作了，但是没有动画，看起来不够炫酷。没关系，我们审查元素，发现是 `.show` 样式增加的过渡效果，opacity 0 <-> 1，而同时设置 `display` 属性会使得它缺失过渡效果。那很简单，我们可以用 `watch` 观察选项，侦听 `isShow` 的变化，在它变化后，间隔一点点时间，再触发动画。关闭的动画就要侦听 `transitionend` 事件了，在关闭动画完成后再隐藏这部分元素。修改后的代码是这样的：

```html
<div class="modal fade" :class="isShowClass ? 'show' : ''" :style="isShow ? 'display:block' : ''" id="sales-editor" @click.self="close" @transitionend="hide">
  <!-- 内容 -->
</div>
<div class="modal-backdrop fade" :class="isShowClass ? 'show' : ''" v-show="isShow"></div>
```

``` javascript
let app = new Vue({
  el: '#app',
  data: {
    isShow: false,
    isShowClass: false
  },
  methods: {
    show() {
      this.isShow = true;
    },
    hide() {
      this.isShow = false;
    },
    close() {
      this.isShowClass = false;
    }
  },
  watch: {
    isShow(value) {
      setTimeout(() => {
        this.nextShow = value;
      }, 50);
    }
  }
});
```
## 填入表单

进展的还算顺利，我们现在已经可以正常开关弹窗了。

接下来我要把表单中的其它属性填进去。对于 Vue 来说，这就太简单了，因为 MVVM 框架最擅长的就是把数据和 UI 进行双向绑定，只用非常少的代码。

```html
<form :action="'/api/sales/' + id" method="POST" id="sales-editor-form">
  <input type="hidden" name="user_id" :value="id">
  <div class="form-group">
    <label for="type">职位</label>
    <select class="form-control" name="type" id="type" v-modal="type">
      <option value="1">区域总监</option>
      <option value="2">商务经理</option>
      <option value="3">商务助理</option>
    </select>
  </div>
</form>
```

三两下清洁溜溜。接下来改一下编辑按钮的响应事件：

```javascript
let modal = new Vue(....); // 把 Vue 组件弄出来备好

$('#table').on('click', '.edit-button', event => {
  let id = $(this).data('id');
  let sales = allSales[id];

  modal.id = id;
  modal.type = sales.type;
  modal.isShow = true;
});
```

代码量一下少了好多，真是舒爽！

不过还没完，因为还要把数据提交给服务器，并且适当的显示结果，所以我们继续工作。这次要侦听表单的提交事件，把它拦截下来，将数据用 Ajax 的方式提交，并且显示结果。

```html
<form @submit.prevent="submit">
  <div class="alert" :class="result-status" v-if="result !== null">{{result}}</div>
```

```javascript
// Vue Modal 组件
data: {
  result: null,
  message: ''
}
methods: {
  submit(event) {
    $.ajax(event.target.action, {
      dataType: 'json',
      data: {
        type: this.type
      }
    })
      .then( response => {
        this.result = response.code;
        this.message = response.message;
        this.$emit('saved', this.id, this.type);
      })
      .catch( err => {
        this.result = 500;
        this.message = err.message;
      });
  }
}

// 外面的老列表代码
let allSales = fetchAllSales();
let modal = new Vue(....);

modal.on('saved', (id, type) => {
  allSales[id].type = type;
});
```

在这段代码中，我侦听到 `submit` 事件，然后把用户选择的数据通过 Ajax 上传给服务器。当服务器返回时，再根据返回值，直接在窗口中显示成功或失败提示。接下来，将完成操作的消息广播出去。外界接到消息后，可以进行下一步的处理。

这里大家可以看到我在侦听事件的 `@submit` 后面增加了 `.prevent`，如果你留心的话，上一节还有用到 `@click.self`，这可不是 jQuery 的[事件命名空间](https://api.jquery.com/event.namespace/)，而是 Vue 的[事件修饰符](https://cn.vuejs.org/v2/guide/events.html#事件修饰符)。这是一种语法糖，因为我们经常需要在事件处理函数里进行 `.preventDefault()` 之类的操作，Vue 干脆把它们进行封装，并以显式声明的方式提供给我们。

另外，我在 Ajax 请求当中使用了 Promise 的写法，如果你对 JS 的异步开发还不甚了解，推荐你看我的另一篇文章：[JavaScript 开发全攻略](https://meathill.gitbooks.io/javascript-async-tutorial/content/)
## 小结

初战告捷！

在这一节中，我们对一段老的，依赖 jQuery 和 Bootstrap 的代码进行了重构，使得它现在支持 Vue，并以 Vue 的方式打开关闭以及与服务器交互。

可能会有同学问：这样做，是不是太简单了……

没错，千里之行始于足下。如果我们从头开发一个项目，大可以直接搬来一套 UI 框架，比如 [Element.ui](http://element.eleme.io/#/zh-CN)，然后全都按照他的标准来写。但当我们面对着一个已经上线提供服务的产品，就不得不谨慎行事，通过一点一滴的积累，把它逐步往我们希望的目标改造。

不过大家可以放心：

1. 这个改造只是入门，我们不会就此收手
2. “MVVM 最先进 UI 架构”的优势还会保持很长时间，甚至在新的交互方式（比如语言，读心）普及之前不会被取代，我们大可以把这个改造过程拉长到3~6个月，甚至一年，而不用担心半路又要改到其它方向
3. 当我们学会并施行组件化开发，开发效率会大增
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
第三步：组件化
========

组件是 Vue 最重要的特性。简单来说，任何 Vue 实例都可以视作组件；一个组件可以由多个组件组合而成；所以一个应用，无论规模，都可以视作若干个组件组成的整体。

> 为了接下来的内容，我建议你先读完 Vue [组件的文档](https://cn.vuejs.org/v2/guide/components.html)。

组件的目的是提高代码的复用性。举个例子，前面我们实现了 Modal 组件的 Vue 化，似乎蛮简单的，效果还不错。但是当另一个页面当中也需要这样的 Modal，麻烦就来了。完全复制一遍当然可以，但这明显不是最好的做法。

这种时候，我们就该把它做成组件。因为整个产品框架仍然处于混合运行的状态，所以[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html) 暂时还不容易接入，我们还是选择全局组件吧。

```javascript
Vue.component('my-modal', {
  template: '',
  props: {
    isShow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isShowClass: false,
      result: null,
      message: ''
    }
  }
  methods: {
    submit(event) {
      // 提交
    },
    show() {
      this.isShow = true;
    },
    hide() {
      this.isShow = false;
    },
    close() {
      this.isShowClass = false;
    }
  },
  watch: {
    isShow(value) {
      setTimeout(() => {
        this.nextShow = value;
      }, 50);
    }
  }
})
```

为防止你没有仔细阅读文档，我还是把一些要点提一下。首先，注册全局组件要用 `Vue.component` 方法，注册出来的组件通过在 HTML 里写标签来使用，如：

```html
<my-modal>

</my-modal>
```

其次，组件的 `data` 属性必须是一个函数，然后返回一个包含要用到数据的对象。

最后，任何组件在用的时候都会变成“子组件”，所以这个时候向它传值必须通过 `prop` 属性。在我们的例子中，负责控制 Modal 显示/隐藏的属性是 `isShow`，原先通过 `modal.isShow = true;` 即可将它弹出。如今我们需要把它暴露给父组件，所以放在 `props` 里。

接下来，我还要调整一下它的模板。既然是组件，必须考虑复用，所以内部表单肯定不能写死了。Vue 给我们提供了 Slot（插槽）作为插入外部内容的手段，所以模板可以改成这样：

```html
<!-- 前面不变 -->
<div class="modal-body">
  <slot @submit.prevent="submit"></slot>
</div>
<!-- 后面也不变 -->
```

模板可以放在任何地方，比如统一堆在 index.html 里面，用 `<script type="text/x-template"></script>` 包裹；也可以写在组件的 `template` 属性里，只要能访问到，都不是问题。

这样，我们就可以在别的地方使用这个 Modal 组件了：

```html
<div class="随便什么容器" id="some-vue-app">
  <my-modal @saved="onModalSaved">
    <form action="/api/some/" method="post">
      <!-- 表单内容 -->
    </form>
  </my-modal>
</div>
```

不过使用 Vue 组件的一定是其它 Vue 实例，如果要混合使用的话，也要用 Vue 实例作为中介。
## 单文件组件

单文件组件是更好的选择。它更容易被复用、被修改、被测试。

不过单文件组件更依赖对整个前端工具体系的掌握，你必须会用 Webpack，会配置各种 loader，对于一些初学者可能会比较困难。所以我建议不要着急上单文件组件，干什么事都应该循序渐进，先把 Vue 用好用熟练，解决掉日常用到的问题，再找机会切换到单文件模式就好。

之前的工作当然不是白费的，Vue 的单文件组件也可以正常使用 import、export，所以之前写好的组件可以直接放进来。

比如我们的 Modal，完成之后，将来如果要誊到单文件组件中，只需要在里面引用就好：

```vue
<template>
  ....
</template>
<script src="./my-modal.js"></script>
```
第三步：路由
========

改造路由本身其实并不困难。真正的难点在于，之前的路由，无论是基于 [Backbone.Router](http://backbonejs.org/#Router) 还是基于 [Page.js](https://visionmedia.github.io/page.js/)，都是侦听 URL 变化，然后调用回调函数来处理。而 Vue 官方插件 Vue-Router，则是直接实例化页面组件。

这样导致我们很难渐进式的迁移功能页，必须小心翼翼的把两个路由分开，比如 `/new/path/to/feature/` 交给新路由，其它的交给老路由，等将来彻底迁移重构完毕再把 `new` 去掉。

本身路由的写法直接看[官方文档](https://router.vuejs.org/zh-cn/essentials/getting-started.html)就好了，大约1个小时就能读完，这里就不在赘述。
第五步：使用其它 UI 框架
========

经过不懈的努力，后台项目的改造告一段落。大部分组件都被重构成基于 Vue，有些还被很好的重构成可复用的组件，用在其它项目中。如今，虽然用的还是 Bootstrap，但已经可以把 jQuery 从依赖中拿掉了。

整个系统基于 Vue 全家桶开发，使用 Webpack + Babel 管理，既时髦又高效。我们对 Vue 开发也很熟悉了，日常开发不在话下。

不过从这个时刻起，我们也不需要像以前那样谨小慎微，土啦吧唧的以“够用就行”的标准来写组件。项目重构完成之后，因为 Vue 单文件组件出色的解耦特性，引用外部组件库也是个不错的选择。而且从提升技术的角度，我还是强烈推荐大家用一用别人的框架。

我这方面的经验也不多，就推荐两个吧：

* [Element UI](http://element.eleme.io/#/zh-CN) 由饿了么团队开发维护的组件库
* [iView](https://www.iviewui.com/) 一套基于 Vue.js 的高质量 UI 组件库

推荐这两款组件库的原因除了它们本身质量不错，都是国人开发，中文文档丰富也是重要原因。
总结
=======

其实，动类似的脑筋的人，我肯定不是第一个。比如 [VueStrap](https://github.com/yuche/vue-strap) 和 [VueStrap](https://github.com/wffranco/vue-strap) 这两个项目，但是它代码写的实在太差，功能也不完备，用起来各种不爽，所以我干脆重写了一些。

Vue 比较让我欣赏的一点也是如此：实现组件库很简单，觉得别的库不合适，自己搞也很快。

甚至我认为这才是正道：大部分基础组件，可以依赖 CSS 来实现，又简单又可靠；部分复杂的功能，自己实现有针对性并且能全把握的大型组件。这也是我写作这篇文章的原因。

## 扩展阅读

* [Web前端开发：为何选择MVVM而非MVC](http://www.cnblogs.com/winter-cn/archive/2012/09/16/2687184.html)
* [谈谈UI架构设计的演化](http://www.cnblogs.com/winter-cn/p/4285171.html)
* [和 Vue.js 框架的作者聊聊前端框架开发背后的故事](http://teahour.fm/2015/08/16/vuejs-creator-evan-you.html)

