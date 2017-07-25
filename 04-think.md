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