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