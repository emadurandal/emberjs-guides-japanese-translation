Components allow you to define controls that you can reuse throughout
your application. If they're generic enough, they can also be shared
with others and used in multiple applications.

Componentは、あなたのアプリケーション内で再利用できるコントロールを定義することを可能にします。もし、それらが十分に一般的な設計であれば、それらは他の人と共有したり、複数のアプリケーションで使うこともできます。

To make a reusable control useful, however, you first need to allow
users of your application to interact with it.

しかしながら、再利用可能なコントロールを便利なものにするためには、あなたのアプリケーションのユーザーに、そのコントロールを操作することを最初に許可する必要があります。

You can make elements in your component interactive by using the
`{{action}}` helper. This is the [same `{{action}}` helper you use in
application templates](http://emberjs.com/guides/templates/actions), but it has an
important difference when used inside a component.

`{{action}}`ヘルパーを使うことで、あなたのComponentの中の要素をインタラクティブにすることができます。これは、[アプリケーションテンプレートであなたが使用できるいくつかの`{{action}}`](http://emberjs.com/guides/templates/actions)です。しかし、コンポーネントの内部で使う時では重要な違いがあります。

Instead of sending an action to the template's controller, then bubbling
up the route hierarchy, actions sent from inside a component are sent
directly to the component's `Ember.Component` instance, and do not
bubble.

テンプレートのControllerにActionを送って、ルート階層を上に伝播していくのではなく、Componentの内部から送られてきたActionは、Componentの`Ember.Component`インスタンスに直接送られます。そして、そこからさらに伝播することはありません。

For example, imagine the following component that shows a post's title.
When the title is clicked, the entire post body is shown:

例えば、投稿のタイトルを表示する、次のようなComponentを想像してください。
タイトルをクリックしたとき、完全な投稿のボディが表示されます。

```handlebars
<script type="text/x-handlebars" id="components/post-summary">
  <h3 {{action "toggleBody"}}>{{title}}</h3>
  {{#if isShowingBody}}
    <p>{{{body}}}</p>
  {{/if}}
</script>
```

```js
App.PostSummaryComponent = Ember.Component.extend({
  actions: {
    toggleBody: function() {
      this.toggleProperty('isShowingBody');
    }
  }
});
```
<a class="jsbin-embed" href="http://jsbin.com/yuzena/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

The `{{action}}` helper can accept arguments, listen for different event
types, control how action bubbling occurs, and more.

`{{action}}`ヘルパーは、引数を受け取ったり、異なるイベントタイプをリッスンしたり、どのようにActionの伝播を起こすのかを制御したり、さらに多くのことを行えます。

For details about using the `{{action}}` helper, see the [Actions
section](http://emberjs.com/guides/templates/actions) of the Templates chapter.

`{{action}}`ヘルパーの利用に関する詳細については、テンプレートの章の[Actions section](http://emberjs.com/guides/templates/actions)を参照してください。

(The original document’s commit SHA1: 7fa5e25d9f090004615f4b440aaac713638e38dc)