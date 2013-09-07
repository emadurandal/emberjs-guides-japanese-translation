# DISPLAYING A MODEL'S COMPLETE STATE
# モデルの完了状態の表示

TodoMVC strikes through completed todos by applying a CSS class `completed` to the `<li>` element. Update `index.html` to apply a CSS class to this element when a todo's `isCompleted` property is true:

TodoMVCは、`<li>`要素にCSSクラス`completed`を適用することで、完了済みのTodoを取り消し線を引いて無効にします。`index.html`を更新して、Todoの`isCompleted`プロパティがtrueになった時に、CSSクラスをこの要素に適用するようにします。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<li {{bind-attr class="isCompleted:completed"}}>
  <input type="checkbox" class="toggle">
  <label>{{title}}</label><button class="destroy"></button>
</li>
<!--- ... additional lines truncated for brevity ... -->
```

This code will apply the CSS class `completed` when the todo's `isCompleted` property is `true` and remove it when the property becomes `false`.

このコードは、TodoのisCompletedプロパティが`true`になったとき、CSSクラス`completed`を適用し、`isCompleted`プロパティが`false`になったときに、クラスを削除します。

The first fixture todo in our application has an `isCompleted` property of `true`. Reload the application to see the first todo is now decorated with a strike-through to visually indicate it has been completed.

私たちのアプリケーションの、フィクスチャデータの最初のTodoは`isCompleted`プロパティが`true`になっています。アプリケーションをリロードし、最初のTodoに取り消し線が引かれて、それが完了済みであることが示されていることを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/oKuwomo/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>
  
### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/b15e5deffc41cf5ba4161808c7f46a283dc2277f)
  * [bind-attr API documentation](/api/classes/Ember.Handlebars.helpers.html#method_bind-attr)
  * [bind and bind-attr article by Peter Wagenet](http://www.emberist.com/2012/04/06/bind-and-bindattr.html)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)