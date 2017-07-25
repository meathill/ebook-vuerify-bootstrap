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